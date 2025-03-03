{

"editor.smoothScrolling": true,

"editor.cursorBlinking": "expand",

"editor.cursorSmoothCaretAnimation": "on",

"editor.hover.above": false,

"workbench.list.smoothScrolling": true,

"editor.mouseWheelZoom": true,

"editor.wordWrap": "on",

"editor.lineHeight": 1.5,

"editor.fontSize": 16,

"editor.fontFamily": "Consolas, '等线', monospace",

"editor.fastScrollSensitivity": 10,

"editor.guides.bracketPairs": true,

"editor.bracketPairColorization.enabled": true,

"editor.suggest.snippetsPreventQuickSuggestions": false,

"editor.acceptSuggestionOnEnter": "smart",

"editor.suggestSelection": "recentlyUsedByPrefix",

"editor.suggest.insertMode": "replace",

// 禁止中文黄框高亮

"editor.unicodeHighlight.nonBasicASCII": false,

// 补全括号选择

"editor.autoClosingBrackets": "beforeWhitespace",

"editor.autoClosingDelete": "always",

"editor.autoClosingOvertype": "always",

"editor.autoClosingQuotes": "beforeWhitespace",

// 禁用缩进猜测

"editor.detectIndentation": false,

"editor.tabSize": 2,

"editor.suggest.preview": true,

"window.dialogStyle": "custom",

// Tab 高度紧凑模式

"window.density.editorTabHeight": "compact",

"debug.showBreakpointsInOverviewRuler": true,

// 文件夹紧凑模式

"explorer.compactFolders": true,

"notebook.compactView": true,

// 自动补齐`HTML`尖括号

"editor.linkedEditing": true,

"html.format.wrapAttributes": "preserve",

"html.format.wrapLineLength": 80,

// "editor.rulers": [80],

"html.format.indentHandlebars": true,

// 格式化自动删分号

"javascript.format.semicolons": "remove",

"typescript.format.semicolons": "remove",

// jsx 表达式添加空格

"javascript.format.insertSpaceAfterOpeningAndBeforeClosingJsxExpressionBraces": true,

"typescript.format.insertSpaceAfterOpeningAndBeforeClosingJsxExpressionBraces": true,

// 类型提示

"javascript.inlayHints.enumMemberValues.enabled": true,

"javascript.inlayHints.functionLikeReturnTypes.enabled": false,

"javascript.inlayHints.parameterNames.enabled": "none",

"typescript.inlayHints.enumMemberValues.enabled": true,

"typescript.locale": "zh-CN",

"typescript.updateImportsOnFileMove.enabled": "always",

"javascript.updateImportsOnFileMove.enabled": "always",

"typescript.preferences.preferTypeOnlyAutoImports": true,

"typescript.preferences.includePackageJsonAutoImports": "on",

"javascript.suggest.autoImports": true,

"typescript.suggest.autoImports": true,

"javascript.preferences.quoteStyle": "single",

"typescript.preferences.quoteStyle": "single",

"vue.updateImportsOnFileMove.enabled": true,

"vue.inlayHints.missingProps": true,

"vue.autoInsert.dotValue": true,

"vue.format.wrapAttributes": "preserve",

"[vue]": {

"editor.defaultFormatter": "Vue.volar"

},

"vue.inlayHints.vBindShorthand": true,

"vue.server.hybridMode": true,

"files.autoGuessEncoding": true,

// 保存自动删除末尾空格

"files.trimTrailingWhitespace": true,

// 路径映射

"path-autocomplete.pathMappings": {

// vite

"/": "${workspace}/public",

// flutter

"lib": "${workspace}/lib",

// Monorepo 要放在前面

"@": {

"conditions": [

{

"when": "**/packages/client/**",

"value": "${workspace}/packages/client/src"

},

{

"when": "**",

"value": "${workspace}/src"

}

]

}

},

// 导入自动补全后缀

"path-autocomplete.extensionOnImport": true,

"path-autocomplete.excludedItems": {

// 忽略 sourcemap

"**/*.map": {

"when": "**"

},

"**/{.git,node_modules}": {

"when": "**"

},

// 不补齐 .ts 后缀

"**/*.ts": {

"when": "**",

"context": "import.*"

},

},

// 搜索吸附目录

"search.searchEditor.singleClickBehaviour": "peekDefinition",

"editor.stickyScroll.enabled": true,

"workbench.tree.enableStickyScroll": true,

// 终端配置

"terminal.integrated.fontFamily": "BitstromWera Nerd Font Mono",

"terminal.integrated.stickyScroll.enabled": true,

"terminal.integrated.profiles.windows": {

"PowerShell7": {

"path": "C:/Develop/PowerShell-7.4.6-win-x64/pwsh.exe"

}

},

"terminal.integrated.defaultProfile.windows": "PowerShell7",

"terminal.integrated.cursorBlinking": true,

"terminal.integrated.cursorWidth": 2,

"terminal.integrated.cursorStyle": "line",

"terminal.integrated.rightClickBehavior": "copyPaste",

"terminal.integrated.gpuAcceleration": "on",

"accessibility.signals.terminalCommandFailed": {

"sound": "auto",

"announcement": "auto"

},

"workbench.iconTheme": "material-icon-theme",

"workbench.list.fastScrollSensitivity": 10,

"workbench.colorTheme": "Pretty Dark Theme",

"workbench.activityBar.location": "bottom",

"editor.foldingImportsByDefault": true,

// index 替换成 目录名

"workbench.editor.customLabels.patterns": {

"**/index.vue": "${dirname}.vue",

"**/index.js": "${dirname}.js",

"**/index.ts": "${dirname}.ts",

"**/index.jsx": "${dirname}.jsx",

"**/index.tsx": "${dirname}.tsx"

},

"workbench.startupEditor": "none",

// java

"java.jdt.ls.java.home": "C:/Develop/jdk21",

"java.configuration.runtimes": [

{

"name": "JavaSE-21",

"path": "C:/Develop/jdk21",

"default": true

}

],

"spring-boot.ls.java.home": "C:/Develop/jdk21",

"maven.executable.path": "C:/Develop/apache-maven-3.9.6/bin/mvn",

"markdown.experimental.updateLinksOnPaste": true,

"[markdown]": {

"files.trimTrailingWhitespace": false

},

"markdown-preview-enhanced.revealjsTheme": "moon.css",

"markdown-preview-enhanced.previewTheme": "solarized-dark.css",

"markdown-preview-enhanced.mermaidTheme": "dark",

"markdown-preview-enhanced.codeBlockTheme": "github-dark.css",

"markdown.copyFiles.destination": {

"**/*": "${documentDirName}/docsAssets/"

},

// 行内样式代码补全

"editor.quickSuggestions": {

"other": true,

"comments": true,

"strings": true

},

"liveServer.settings.donotShowInfoMsg": true,

"editor.wordSeparators": "`~!@%^&*()=+[{]}\\|;:'\",.<>/?（），。；：",

"editor.minimap.enabled": false,

"editor.foldingStrategy": "indentation",

// JSX

"emmet.triggerExpansionOnTab": true,

"emmet.includeLanguages": {

"javascript": "javascriptreact",

"typescript": "javascriptreact"

},

"emmet.excludeLanguages": [

"javascript",

"typescript"

],

"search.followSymlinks": false,

"liveServer.settings.donotVerifyTags": true,

"update.mode": "manual",

"search.exclude": {

"**/node_modules": true,

"**/pnpm-lock.yaml": true,

"**/package-lock.json": true,

"**/.DS_Store": true,

"**/.git": true,

"**/.gitignore": true,

"**/.idea": true,

"**/.svn": true,

"**/.vscode": true,

"**/build": true,

"**/dist": true,

"**/tmp": true,

"**/yarn.lock": true

},

// 配置文件关联

"files.associations": {

// 比如小程序中的 .wxss 这种文件，会把它作为css文件来处理，提供对应的css的语法提示，css的格式化等

"*.wxss": "css",

"*.wxml": "html",

"*.svg": "html",

"*.xml": "html",

"*.wxs": "javascript",

// json注释

"*.cjson": "jsonc",

"*.json": "jsonc"

},

"css.lint.unknownAtRules": "ignore",

"scss.lint.unknownAtRules": "ignore",

"less.lint.unknownAtRules": "ignore",

"togglequotes.chars": [

"'",

"`"

],

"explorer.copyRelativePathSeparator": "/",

// 大纲

"notebook.outline.showCodeCellSymbols": false,

"outline.showArrays": false,

"outline.showBooleans": false,

"outline.showConstants": false,

"outline.showNull": false,

"outline.showNumbers": false,

"outline.showObjects": false,

"outline.showOperators": false,

"outline.showPackages": false,

"outline.showStructs": false,

"outline.showEvents": false,

"outline.showFields": false,

"outline.showFiles": false,

"outline.showProperties": false,

"outline.showEnumMembers": false,

"outline.showEnums": false,

"outline.showInterfaces": false,

"outline.showKeys": false,

"outline.showTypeParameters": false,

"create-uniapp-view.directory": true,

"create-uniapp-view.style": "scss",

"create-uniapp-view.template": "vue3",

"create-uniapp-view.name": "与文件夹同名",

// todo-tree 标签配置 标签兼容大小写字母

"todo-tree.regex.regex": "((%|#|//|<!--|^\\s*\\*)\\s*($TAGS)|^\\s*- \\[ \\])",

"todo-tree.filtering.excludeGlobs": [

"/node_modules/*/**"

],

"todo-tree.general.tags": [

"@todo",

"@bug"

],

"todo-tree.regex.regexCaseSensitive": false,

"todo-tree.highlights.defaultHighlight": {

// 如果相应变量没赋值就会使用这里的默认值

"foreground": "#000", // 字体颜色

"background": "#181818", // 背景色

"icon": "check", // 标签样式 check 是一个对号的样式

"type": "tag" // 填充色类型 可在TODO TREE 细节页面找到允许的值

},

"todo-tree.highlights.customHighlight": {

"@todo": {

"icon": "alert", // 标签样式

"background": "#F9D569", // 背景色

"rulerColour": "#F9D569", // 外框颜色

"iconColour": "#F9D569" // 标签颜色

},

"@bug": {

"background": "#e75064",

"icon": "bug",

"rulerColour": "#e75064",

"iconColour": "#e75064"

}

},

"todo-tree.general.rootFolder": "${workspaceFolder}/src",

"todo-tree.filtering.useBuiltInExcludes": "search excludes",

"git.enableSmartCommit": true,

"security.promptForLocalFileProtocolHandling": false,

"fittencode.languagePreference.displayPreference": "zh-cn",

"fittencode.languagePreference.commentPreference": "zh-cn"

}