# 南京大学本科毕业论文样式指南
根据南京大学本科生院的论文模板，讲解毕业论文WORD排版风格。文档采用南京大学Linux用户组开发的论文模板[NJUThesis](https://github.com/nju-lug/NJUThesis)撰写。

# 使用方法
下载PDF文档观看即可。

# 没事的人可以看看

文中涉及的回归结果复现代码如下：

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
 indicate("年份效应 =*.t", labels(YES NO) )  ///存在i.t变量输出YES,否则输出NO.
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



# 致谢

感谢南京大学Linux用户组的 atxy-blip 在github上解答疑问！


