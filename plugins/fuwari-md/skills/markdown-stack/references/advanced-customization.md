# Advanced Customization Guide

This guide covers advanced customization scenarios beyond basic configuration.

## Creating Custom Directives

### Step 1: Define Directive Syntax

Choose your directive type:

| Type | Syntax | Use Case |
|------|--------|----------|
| Container | `:::name[label]\ncontent\n:::` | Blocks with children (callouts, tabs) |
| Leaf | `::name{attr="value"}` | Self-closing embeds (video, tweet) |
| Text | `:name[inline content]` | Inline elements (spoilers, tooltips) |

### Step 2: Create the Remark Parser

For directives to work, you need `remark-directive` and a custom parser:

```javascript
// plugins/remark-custom-directive.js
import { visit } from 'unist-util-visit';
import { h } from 'hastscript';

export function remarkCustomDirective() {
  return (tree) => {
    visit(tree, (node) => {
      if (
        node.type === 'containerDirective' ||
        node.type === 'leafDirective' ||
        node.type === 'textDirective'
      ) {
        // Convert directive to hast node
        const data = node.data || (node.data = {});
        const tagName = node.type === 'textDirective' ? 'span' : 'div';

        data.hName = tagName;
        data.hProperties = h(tagName, {
          ...node.attributes,
          'data-directive': node.name,
          'data-directive-type': node.type.replace('Directive', ''),
        }).properties;
      }
    });
  };
}
```

### Step 3: Create the Rehype Component

```javascript
// plugins/rehype-component-custom.mjs
import { h } from 'hastscript';

/**
 * Custom YouTube embed directive
 * Usage: ::youtube{id="dQw4w9WgXcQ"}
 */
export function YoutubeComponent(properties, children) {
  // Leaf directives should have no children
  if (Array.isArray(children) && children.length !== 0) {
    return h('div', { class: 'hidden' }, 'youtube must be a leaf directive');
  }

  const { id, title = 'YouTube Video' } = properties;

  if (!id) {
    return h('div', { class: 'directive-error' }, 'Missing video ID');
  }

  return h('div', { class: 'youtube-embed' }, [
    h('iframe', {
      src: `https://www.youtube.com/embed/${id}`,
      title,
      frameborder: '0',
      allow: 'accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture',
      allowfullscreen: true,
      loading: 'lazy',
      style: 'width: 100%; aspect-ratio: 16/9;',
    }),
  ]);
}
```

### Step 4: Register the Component

```javascript
// In your config
import { YoutubeComponent } from './plugins/rehype-component-custom.mjs';

export default {
  markdown: {
    rehypePlugins: [
      [rehypeComponents, {
        components: {
          youtube: YoutubeComponent,
          // Add more custom components here
        },
      }],
    ],
  },
};
```

### Complete Example: Tabs Component

```javascript
// plugins/rehype-component-tabs.mjs
import { h } from 'hastscript';

/**
 * Tabs container directive
 * Usage:
 * :::tabs
 * ::tab[Label 1]
 * Content 1
 * ::tab[Label 2]
 * Content 2
 * :::
 */
export function TabsComponent(properties, children) {
  const tabs = [];
  const panels = [];
  let currentTab = null;
  let currentPanel = [];

  // Parse children into tabs and panels
  for (const child of children) {
    if (child.tagName === 'div' && child.properties?.['data-directive'] === 'tab') {
      if (currentTab) {
        panels.push(h('div', { class: 'tab-panel', role: 'tabpanel' }, currentPanel));
      }
      currentTab = child.children?.[0]?.value || `Tab ${tabs.length + 1}`;
      tabs.push(currentTab);
      currentPanel = [];
    } else if (currentTab) {
      currentPanel.push(child);
    }
  }

  // Add last panel
  if (currentTab) {
    panels.push(h('div', { class: 'tab-panel', role: 'tabpanel' }, currentPanel));
  }

  const tabId = `tabs-${Math.random().toString(36).slice(2, 8)}`;

  return h('div', { class: 'tabs-container', id: tabId }, [
    h('div', { class: 'tab-list', role: 'tablist' },
      tabs.map((label, i) =>
        h('button', {
          class: i === 0 ? 'tab-button active' : 'tab-button',
          role: 'tab',
          'aria-selected': i === 0 ? 'true' : 'false',
          'data-tab-index': i,
        }, label)
      )
    ),
    h('div', { class: 'tab-panels' },
      panels.map((panel, i) => ({
        ...panel,
        properties: {
          ...panel.properties,
          'data-tab-index': i,
          style: i === 0 ? '' : 'display: none;',
        },
      }))
    ),
    // Client-side JavaScript for interactivity
    h('script', {}, `
      (function() {
        const container = document.getElementById('${tabId}');
        container.querySelectorAll('.tab-button').forEach(btn => {
          btn.addEventListener('click', () => {
            const index = btn.dataset.tabIndex;
            container.querySelectorAll('.tab-button').forEach(b => {
              b.classList.remove('active');
              b.setAttribute('aria-selected', 'false');
            });
            btn.classList.add('active');
            btn.setAttribute('aria-selected', 'true');
            container.querySelectorAll('.tab-panel').forEach((p, i) => {
              p.style.display = i == index ? '' : 'none';
            });
          });
        });
      })();
    `),
  ]);
}
```

## Extending Expressive Code

### Custom Plugin Structure

```typescript
// plugins/expressive-code-custom.ts
import { definePlugin } from '@expressive-code/core';

export const pluginCustomFeature = () => {
  return definePlugin({
    name: 'custom-feature',

    // Base styles (always applied)
    baseStyles: `
      .custom-feature {
        /* your styles */
      }
    `,

    // Process code blocks
    hooks: {
      preprocessCode: ({ codeBlock }) => {
        // Modify code before rendering
        // Example: add line numbers to first line
        if (codeBlock.meta.includes('numbered')) {
          codeBlock.props.showLineNumbers = true;
        }
      },

      postprocessRenderedBlock: ({ codeBlock, renderData }) => {
        // Modify rendered HTML
        // Example: add custom wrapper
        const wrapper = renderData.blockAst;
        wrapper.properties.className = [
          ...(wrapper.properties.className || []),
          'custom-wrapper',
        ];
      },
    },
  });
};
```

### Adding Custom Meta Properties

```typescript
// Support custom meta like: ```js custom-prop="value"
import { definePlugin } from '@expressive-code/core';

export const pluginCustomMeta = () => {
  return definePlugin({
    name: 'custom-meta',
    hooks: {
      preprocessMetadata: ({ codeBlock }) => {
        // Parse custom meta
        const match = codeBlock.meta.match(/custom-prop="([^"]+)"/);
        if (match) {
          codeBlock.props.customProp = match[1];
        }
      },
    },
  });
};
```

### Custom Language Badge Colors

```typescript
// plugins/custom-language-colors.ts
import { definePlugin } from '@expressive-code/core';

const languageColors: Record<string, string> = {
  typescript: '#3178c6',
  javascript: '#f7df1e',
  python: '#3776ab',
  rust: '#dea584',
  go: '#00add8',
  // Add your custom colors
};

export const pluginLanguageColors = () => {
  return definePlugin({
    name: 'language-colors',
    baseStyles: Object.entries(languageColors)
      .map(([lang, color]) => `
        .expressive-code[data-language="${lang}"] .language-badge {
          background: ${color};
          color: ${getContrastColor(color)};
        }
      `)
      .join('\n'),
  });
};

function getContrastColor(hex: string): string {
  const r = parseInt(hex.slice(1, 3), 16);
  const g = parseInt(hex.slice(3, 5), 16);
  const b = parseInt(hex.slice(5, 7), 16);
  const luminance = (0.299 * r + 0.587 * g + 0.114 * b) / 255;
  return luminance > 0.5 ? '#000' : '#fff';
}
```

## Theme Customization Beyond CSS Variables

### Dynamic Theme Switching

```javascript
// Dynamic hue rotation based on page
function setPageHue(hue) {
  document.documentElement.style.setProperty('--hue', hue);
}

// Per-category colors
const categoryHues = {
  technology: 250,  // Blue
  design: 320,      // Pink
  tutorial: 180,    // Cyan
  news: 30,         // Orange
};

// Apply based on current page category
const category = document.body.dataset.category;
if (categoryHues[category]) {
  setPageHue(categoryHues[category]);
}
```

### Seasonal Themes

```css
/* Holiday theme overrides */
.theme-holiday {
  --hue: 0;  /* Red */
  --primary: oklch(0.55 0.25 0);
  --card-bg: oklch(0.98 0.01 120);  /* Light green tint */
}

.theme-holiday::before {
  content: "❄️";
  position: fixed;
  animation: snow 10s linear infinite;
}
```

### OKLCH Color Palette Generator

```javascript
// Generate harmonious color palette from base hue
function generatePalette(baseHue) {
  return {
    primary: `oklch(0.6 0.15 ${baseHue})`,
    secondary: `oklch(0.5 0.12 ${(baseHue + 30) % 360})`,
    accent: `oklch(0.7 0.2 ${(baseHue + 180) % 360})`,
    muted: `oklch(0.4 0.05 ${baseHue})`,
  };
}

// Apply palette
const palette = generatePalette(250); // Blue-based
Object.entries(palette).forEach(([name, value]) => {
  document.documentElement.style.setProperty(`--color-${name}`, value);
});
```

## Custom Component Registration

### Global Component Registry

```javascript
// plugins/component-registry.js
const componentRegistry = new Map();

export function registerComponent(name, handler) {
  componentRegistry.set(name, handler);
}

export function getComponent(name) {
  return componentRegistry.get(name);
}

export function getAllComponents() {
  return Object.fromEntries(componentRegistry);
}

// Usage in config
import { getAllComponents } from './plugins/component-registry.js';
import { registerComponent } from './plugins/component-registry.js';

// Register components
registerComponent('youtube', YoutubeComponent);
registerComponent('tweet', TweetComponent);
registerComponent('codepen', CodepenComponent);

// Use in rehype config
[rehypeComponents, { components: getAllComponents() }]
```

### Lazy-Loading Components

```javascript
// For heavy components, load on demand
export async function LazyChartComponent(properties, children) {
  const { h } = await import('hastscript');
  const Chart = await import('./heavy-chart-lib.js');

  // Render placeholder with loading script
  return h('div', {
    class: 'chart-container',
    'data-chart-config': JSON.stringify(properties),
  }, [
    h('div', { class: 'chart-loading' }, 'Loading chart...'),
    h('script', {
      type: 'module',
      src: '/scripts/init-charts.js',
    }),
  ]);
}
```

## Testing Custom Components

### Unit Testing with Vitest

```typescript
// tests/components.test.ts
import { describe, it, expect } from 'vitest';
import { YoutubeComponent } from '../plugins/rehype-component-custom.mjs';

describe('YoutubeComponent', () => {
  it('renders iframe with correct src', () => {
    const result = YoutubeComponent({ id: 'abc123' }, []);
    expect(result.children[0].properties.src).toBe(
      'https://www.youtube.com/embed/abc123'
    );
  });

  it('returns error for missing id', () => {
    const result = YoutubeComponent({}, []);
    expect(result.properties.class).toBe('directive-error');
  });
});
```

### Integration Testing

```javascript
// tests/integration.test.js
import { unified } from 'unified';
import remarkParse from 'remark-parse';
import remarkDirective from 'remark-directive';
import remarkRehype from 'remark-rehype';
import rehypeStringify from 'rehype-stringify';

const processor = unified()
  .use(remarkParse)
  .use(remarkDirective)
  .use(remarkCustomDirective)
  .use(remarkRehype)
  .use(rehypeComponents, { components: { youtube: YoutubeComponent } })
  .use(rehypeStringify);

it('transforms youtube directive', async () => {
  const input = '::youtube{id="abc123"}';
  const result = await processor.process(input);
  expect(String(result)).toContain('youtube.com/embed/abc123');
});
```

## Debugging Tips

### Enable Verbose Logging

```javascript
// Add debug plugin to see transformations
function debugPlugin(name) {
  return () => (tree, file) => {
    console.log(`[${name}]`, JSON.stringify(tree, null, 2).slice(0, 500));
  };
}

remarkPlugins: [
  debugPlugin('after-parse'),
  remarkDirective,
  debugPlugin('after-directive'),
  // ...
]
```

### AST Explorer

Use https://astexplorer.net/ with:
- Parser: `remark` (for markdown)
- Transform: Your custom plugin

This helps visualize how your plugins modify the AST.
