## install fecs check on visual studio code

### 请先安装 fecs

> npm install fecs -g

![](https://raw.githubusercontent.com/21paradox/fecs-visual-studio-code/master/img/install-fecs.png)

没有npm? 点击http://nodejs.org/download/ 下载

### 打开 visual studio code

没有 visual studio code? 点击 https://code.visualstudio.com/ 下载

### 按住 ctrl + shift + p 调出控制台， 输入 task, 选择 Configure Task Runner

![](https://raw.githubusercontent.com/21paradox/fecs-visual-studio-code/master/img/ctrl+shift+p.png)

### 把相应的 json 配置文件替换成如下信息

![](https://raw.githubusercontent.com/21paradox/fecs-visual-studio-code/master/img/replace-selectedWords.png)

```js

{
	"version": "0.1.0",
	"command": "fecs check", // 这里是全局 fecs命令
	"isShellCommand": true,
	
	// task 可以有多个，定义不同的 taskName 和 args就行
	"tasks": [
		{
			"taskName": "check my code01" , 
			
			// 这里填写 path, 使用 node-glob 的match方式， 
			// **/* 代表当前目录 和子目录 下面所有的文件
			// node-glob 也可以 match 更复杂的
			// 比如 不检查script下 test文件夹 和 所有 test/spec 文件, 可以写：
			// script/!(test)/**/!(*_test.js|*_spec.js)'
			// 更多 参考  https://github.com/isaacs/node-glob
			"args": ["**/*"], 
			
			// Make this the default build command.
			"isBuildCommand": true,
			// Show the output window only if unrecognized errors occur.
			"showOutput": "always",
			
			"problemMatcher": {
				// The problem is owned by the typescript language service. Ensure that the problems
				// are merged with problems produced by Visual Studio's language service.
				"owner": "javascript",
				// The file name for reported problems is relative to the current working directory.
				"fileLocation": ["relative", "${cwd}"],
				// The actual pattern to match problems in the output.
				"pattern": [
					{
			           "regexp": "^fecs\\s+?INFO\\s(.+?)\\s.+messages\\)$", //regex 检测文件名
			           "file": 1
			        },
					{
						// The regular expression. Matches HelloWorld.ts(2,10): error TS2339: Property 'logg' does not exist on type 'Console'.
						"regexp": "^fecs\\s+?(.*)\\s→\\sline\\s(\\d+),\\scol\\s(\\d+):\\s(.*)", //regex 检测error
						// The match group that denotes the file containing the problem.
						"severity": 1,
						// The match group that denotes the problem location.
						"line": 2,
						// The match group that denotes the problem's severity. Can be omitted.
						"column": 3,
						// The match group that denotes the problem code. Can be omitted.
						"message": 4,
						// The match group that denotes the problem's message.
						"loop": true
					}
				]
			}
		}
	]
}

```


### 再次 按住 ctrl + shift + p 调出控制台， 输入 task, 选择你的task, 开始

![](https://raw.githubusercontent.com/21paradox/fecs-visual-studio-code/master/img/after-replace.png)


