# svgr.d.ts

- https://velog.io/@yejiiha/Next.js-svgrwebpack-Cannot-find-module-%EC%97%90%EB%9F%AC

```json
"next": "15.1.4",
"next-svgr": "^0.0.2",
```

```typescript
declare module '*.svg' {
  import type { FC, SVGProps } from 'react';

  const content: FC<SVGProps<SVGElement>>;
  export default content;
}

declare module '*.svg?url' {
  const content: any;
  export default content;
}
```

- next.config.mjs

```javascript 
const nextConfig = {
    webpack(config) {
        config.cache = {
            type: 'filesystem',
            compression: 'gzip',
            allowCollectingMemory: true,
        };

        const fileLoaderRule = config.module.rules.find((rule) => rule.test?.test?.('.svg'));

        config.module.rules.push(
            {
                ...fileLoaderRule,
                resourceQuery: /url/, // *.svg?url
                test: /\.svg$/i,
            },
            {
                issuer: fileLoaderRule.issuer,
                resourceQuery: { not: [...fileLoaderRule.resourceQuery.not, /url/] },
                test: /\.svg$/i,
                use: ['@svgr/webpack'],
            }
        );

        return config;
    },
    experimental: {
        turbo: {
            resolveAlias: {
                canvas: './empty-module.ts',
            },
            rules: {
                '*.svg': {
                    loaders: ['@svgr/webpack'],
                    as: '*.js',
                },
            },
        },
    },
};
```

- tsconfig.json

```json
{
  "compilerOptions": {
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./*"],
      "@icons/*": ["./public/images/icons/*"],
      "@modal/*": ["./components/Modal/ui/*"]
    }
  },
  "include": [
    "svgr.d.ts",
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx",
    ".next/types/**/*.ts",
    "app/**/*"
  ],
  "exclude": ["node_modules"]
}
```