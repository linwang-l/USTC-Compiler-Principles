# Lab1实验说明

    首先要完成的是answer.pdf，主要需要了解的是正则表达式，Flex，Bison。

    实验文档见[Lab1 简介 - USTC 编译原理和技术 2025](https://ustc-compiler-2025.github.io/homepage/lab1/)

    正则表达式直接查看lab1简介的介绍即可。

    Flex可以通过lab1文档中的介绍查看`info flex`，也可以从[Flex 的词法分析，适用于 Flex 2.6.2：顶部](https://westes.github.io/flex/manual/?utm_source=chatgpt.com)查看，中文文档可以查看[Flex中文文档 | Sakuraのblog](https://eternalsakura13.com/2020/05/27/flex/?utm_source=chatgpt.com)。

    Bison的内容可以查看lab1简介，需要注意的是**Bison 的语法树是按自下而上的归约方式进行构建的**。

    answer的内容在另一个文件里，可能有误，仅供参考。

### Lab1 Part1

    首先先把该拉取的拉取下来。

```bash
git remote -v
git checkout
```

     这两个命令一个是用来查看远程的上下游是哪里，一个是查看自己现在所处于的分支。

    这里建议在gitlab里先fork一个属于自己的库，然后clone下自己的库。clone的编译原理助教的库的话记得修改远程仓库对应是链接，不然修改会传到助教的仓库里。

    可以采用

```bash
git fetch origin 
git checkout lab1 
git pull
```

    完成对lab1分支的切换与拉取（已经包含拉取后的合并操作）。

> **推荐同学们下载 vscode 的插件 lex 和 bison，可以在补全 `.l`文件和 `.y`文件的时候提供高亮。**

请一定注意下载，因为我没下载，摸黑做的，眼睛快瞎了，除此之外，记得细心看文档，需要修改两个文件——`lexical_analyzer.l`和`syntax_analyzer.y`。

    其中在lab1中，需要完成对syntax_analyzer.y的修改，直接抄[cminusf.pdf](https://ustc-compiler-2025.github.io/homepage/lab1/assets/cminusf.pdf)即可（这是助教给的产生式对应关系，Bison格式）。

    `lexical_analyzer.l`需要完成以下内容的定义，采用的是Flex的格式

`"else"      { pos_start = pos_end; pos_end += 4; pass_node("else"); return ELSE; }
"if"        { pos_start = pos_end; pos_end += 2; pass_node("if"); return IF; }
"int"       { pos_start = pos_end; pos_end += 3; pass_node("int"); return INT; }
"return"    { pos_start = pos_end; pos_end += 6; pass_node("return"); return RETURN; 
"void"      { pos_start = pos_end; pos_end += 4; pass_node("void"); return VOID; }
"while"     { pos_start = pos_end; pos_end += 5; pass_node("while"); return WHILE; }
"float"     { pos_start = pos_end; pos_end += 5; pass_node("float"); return FLOAT; }
"main"      { pos_start = pos_end; pos_end += 4; pass_node("main"); return IDENTIFIER; } `

`[ \t]+   { pos_start = pos_end; pos_end += yyleng; /* skip spaces and tabs */ }
\n       { lines++; pos_start = pos_end; pos_end = 1; /* skip newline */ }`

`\[      { pos_start = pos_end; pos_end += 1; pass_node(yytext); return LBRACKET; }
\]      { pos_start = pos_end; pos_end += 1; pass_node(yytext); return RBRACKET; }
\( { pos_start = pos_end; pos_end += 1; pass_node(yytext); return LPARENTHESE; }
\) { pos_start = pos_end; pos_end += 1; pass_node(yytext); return RPARENTHESE; }
\{ { pos_start = pos_end; pos_end += 1; pass_node(yytext); return LBRACE; }
\} { pos_start = pos_end; pos_end += 1; pass_node(yytext); return RBRACE; }
"/*"([^*]|\*+[^*/])*\*+"/"   { /* 忽略注释 */ }
[a-zA-Z]+       { pass_node(yytext); return IDENTIFIER; }      /* ID */
[0-9]+          { pass_node(yytext); return INTEGER; }         /* INTEGER */
([0-9]+\.[0-9]*|[0-9]*\.[0-9]+) { pass_node(yytext); return FLOATPOINT; } /* FLOAT */`

---

这样，Part1部分就写好了。

进入`tests/1-parser`运行`eval_phase1.sh`脚本即可，评测全部通过就可以提交。

参数可以是`-all`。




