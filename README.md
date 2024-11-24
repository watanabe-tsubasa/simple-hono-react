# simple-hono-react

## 動かし方

```txt
bun install
bun run dev
```

```txt
bun run deploy
```

## 背景

- [zenn](https://zenn.dev/yusukebe/articles/06d9cc1714bfb7)
- [zenn2](https://zenn.dev/laiso/articles/c7eba95ce43feb)

これをみて、

```tsx
import { Hono } from 'hono'
import { renderToString } from 'react-dom/server'

const app = new Hono()

// APIエンドポイント
app.get('/api/clock', (c) => {
  return c.json({
    time: new Date().toLocaleTimeString(),
  })
})

// 全てのその他のルート
app.get('*', (c) => {
  // 環境に応じてスクリプトパスを切り替え
  const scriptPath = import.meta.env.PROD
    ? '/static/client.js'
    : '/src/client.tsx'

  // HTMLを文字列として生成
  const htmlContent = renderToString(
    <>
      <html>
        <head>
          <meta charSet="utf-8" />
          <meta content="width=device-width, initial-scale=1" name="viewport" />
          <link
            rel="stylesheet"
            href="https://cdn.simplecss.org/simple.min.css"
          />
          <script type="module" src={scriptPath}></script>
        </head>
        <body>
          <div id="root"></div>
        </body>
      </html>
    </>
  )

  return c.html(htmlContent)
})

export default app
```

gptにコード直させたら動いた
