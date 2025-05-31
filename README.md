# 南京大学本科毕业论文样式指南
根据南京大学本科生院的论文模板，本文档拓展讲解毕业论文排版常见问题。文档采用南京大学Linux用户组开发的论文模板[NJUThesis](https://github.com/nju-lug/NJUThesis)撰写。

# 使用方法
下载PDF文档观看即可。WORD排版者可以忽略下面的代码。


# 没事的人可以看看

文中涉及的回归结果表格制作的stata代码如下：

```
// 回归并保存结果
global xlist ed fem blk exp exp2 wks ms union occ south smsa ind
regress lwage $xilist i.t, vce(cluster id)
estimates store OLS
xtreg lwage $xilist, be vce(bootstrap, reps(1000))
estimates store BE
xtreg lwage $xilist, fe vce(robust)
estimates store FE
xtreg lwage $xilist, re vce(robust)
estimates store RE
xthtaylor lwage $xilist, endog(ed exp exp2 wks ms union) vce(robust)
estimates store HT

// 五个回归结果列表输出到results.rtf
esttab OLS BE FE RE HT using results.rtf, ///
 b(4) se(4)                   ///系数和标准误保留4位小数
 star(* 0.1 ** 0.05 *** 0.01) ///设置显著性程度标注符号
 scalars(r2 r2_o r2_b r2_w sigma_u sigma_e rho N) ///选择输出标量
 sfmt(4)                    ///scalars显示4位小数
 indicate("年份 =*.t", labels(YES NO) )  ///存在i.t变量输出YES,否则输出NO.
 mtitle(POOL BE FE RE HT)   ///模型标题
 title("面板模型估计量比较")  ///表格标题
 varwidth(17)               ///变量列宽17位字符
 modelwidth(7)              ///模型列宽7位
 alignment(c)               ///模型列居中对齐
 obslast nogaps compress replace   ///
 order(ed fem blk exp exp2 wks ms union occ south smsa ind) /// 设定变量显示顺序
 coeflabels(ed ED(教育) fem FEM(女性)  ///修改变量标签，代替变量出现在变量列
   blk BLK(黑人) exp EXP(工作经验) exp2 EXP^2 wks WKS(工作周数)  ///
   ms MS(婚姻状况) union UNION(加入工会) occ OCC(蓝领) ///
   south SOUTH(居住南方) smsa SMSA(居住大城市) ind IND(制造业) _cons 常数项)  ///
 addnotes("添加自定义表注，符合自明原则。")
 ```

本文档使用的常见NJUThesis设置如下：

```
% 标题格式设置
\njusetformat{chapter}{\zihao{3}\sffamily\centering}%一级标题改为三号
\ctexset{%重现学校模板章节间距，基本一致。
  chapter/number        = \arabic{chapter},  %第一章改为第1章
  chapter/beforeskip    = 0pt,           
  chapter/afterskip     = 23.4pt,        
  section/beforeskip    = 7.8pt,         
  section/afterskip     = 0pt,           
  subsection/beforeskip = 0pt,           
  subsection/afterskip  = 0pt,           
}

% 符号表的居中大标题（默认四号黑体）修改为三号宋体加粗，与目录页标题一致
\begingroup
\ctexset{chapter/format=\centering\zihao{3}\normalfont\bfseries}
\begin{notation}[10cm]
  \item[TNR]  泰晤士新罗马体(Times New Roman)
\end{notation}
\endgroup

% 脚注文本修改为五号。默认小五号
\renewcommand{\footnotesize}{\zihao{5}}

% 参考文献格式设置
\njusetup[bib]{
    style = author-year,
    resource = {mybibfile.bib},
    option = {
        doi           = false,     %关闭DOI字段信息
        isbn          = false,     %关闭ISBN字段信息
        url           = false,     %关闭URL字段信息
        eprint        = false,     %关闭EPRINT字段信息
        gbtype        = true,      %显示文献标识代码
        gbpunctin     = false,     %析出文献符号//替换为：in/见
        gbnamefmt     = uppercase, %默认。姓名首字母大写用lowercase
        maxbibnames   = 99,        %作者超过99位只列出前99位
        minbibnames   = 99,        
        maxcitenames  = 2,         %引用标签超过2个作者只显示第一作者
        mincitenames  = 1,
      }
}

% 中文参考文献作者之间逗号改为顿号
\DeclareDelimFormat[bib]{multinamedelim}
  {\iffieldequalstr{userf}{chinese}{、}{\addcomma\addspace}}
\DeclareDelimFormat[bib]{finalnamedelim}
  {\iffieldequalstr{userf}{chinese}{、}{\addcomma\addspace}}
% 中文引文标签作者之间逗号改为顿号
\DeclareDelimFormat[cite,parencite,textcite]{multinamedelim}
  {\iffieldequalstr{userf}{chinese}{、}{\addcomma\addspace}}
% 引文标签最后一个作者前，中文用“和”，英文用“and”。事实上对应两位作者的情况。
\DeclareDelimFormat[cite,parencite,textcite]{finalnamedelim}
  {\iffieldequalstr{userf}{chinese}{和}{\addspace and \addspace}}

% 隐藏参考文献的一些字段信息
\DeclareFieldFormat[patent]{type}{} 
\DeclareFieldFormat[online]{pubstate}{} 
\DeclareFieldFormat[book]{pagetotal}{} 

% 参考文献表增加悬挂缩进，并修改文献表的标题。
\setlength{\bibitemindent}{-2em} 
\setlength{\bibhang}{2em} 
\printbibliography[title={参考文献(适用于著者--出版年制)}]

% 定理环境设置
\njusetup[theorem]{
     style         = plain,
     header-font   = \normalfont\bfseries,
     body-font     = \normalfont,
     qed-symbol    = \ensuremath {\mdlgwhtsquare},
     counter       = chapter,
     share-counter = false,
     type          = { 
        {{proof,*+}       {证明:}   },
        {{guiderule}      {规则}    }, 
        {{theorem}        {定理}    },
        {{recommendation} {推荐样式} },
        },
     define
}
% 定理2.1修改为定理2-1，与图表公式编号风格一致。
\renewcommand{\thetheorem}{\thechapter-\arabic{theorem}}
\renewcommand{\theguiderule}{\thechapter-\arabic{guiderule}}
\renewcommand{\therecommendation}{\thechapter-\arabic{recommendation}}
```

# 致谢

感谢南京大学Linux用户组在github上解答疑问！


