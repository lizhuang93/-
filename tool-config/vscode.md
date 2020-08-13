### 1. vscode 开启对 webpack alias(文件别名) 引入的智能提示
1. webpack.config.js 配置别名
```
resolve: {
      extensions: ['.js', '.jsx', '.vue'],
      alias: {
        // 不需要编译器
        vue$: 'vue/dist/vue.runtime.esm.js',
        '@': path.resolve(__dirname, 'src'), // 别名 @ -> src
      },
    },
```
2. 根目录下创建jsconfig.json
```
// jsconfig.json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@/*": [
        "src/*"
      ]
    },
    "target": "ES6",
    "module": "commonjs",
    "jsx": "react", // 重要！支持jsx文件，否则jsx文件无法跳转
    "allowSyntheticDefaultImports": true
  },
  "exclude": [
    "node_modules",
    "dist"
  ]
}
```
3. import someThing from '@/components' 的时候我们的智能提示又回来了。nice。
