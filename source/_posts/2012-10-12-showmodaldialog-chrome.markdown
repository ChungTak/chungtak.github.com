---
layout: post
title: "Chrome下showModalDialog dialogWidth长度属性无效"
date: 2012-10-12 09:29
comments: true
categories: Javascript 
---

在Chrome浏览器下调用window.showModalDialog 会忽略dialogWidth和dialogHeight属性,其他浏览器IE,firefox却显示正常。  
原因是Chrome下dialogWidth和dialogHeight的值**不能带"px"后缀**.正确写法`"dialogWidth:100;dialogHeight:200"`    
但其他浏览器则相反需要带有"px"后缀,例如:`"dialogWidth:100px;dialogHeight:200px"`  

  
要兼容所有浏览器需要判断浏览器类型，参考代码：  
``` javascript  
function ModalDialog(sURL, vArguments, ModalDialogHeight, ModalDialogWidth) {
        var sFeatures;
        if (window.showModalDialog) {//判断浏览器是否支持showModalDialog函数
            if (!navigator.userAgent.toLowerCase().match('chrome')) {//非chrome浏览器添加px
                ModalDialogWidth += "px";
                ModalDialogHeight += "px";
            }
            sFeatures = "status:no;center:yes;dialogWidth:" + ModalDialogWidth + ";dialogHeight:" + ModalDialogHeight + ";resizable:yes;";
            var returnResult = window.showModalDialog(sURL, vArguments, sFeatures);
            if (returnResult) {
                return returnResult;
            }
        }
        else {//使用window.open替代showModalDialog
            sFeatures = "status=no,width=" + ModalDialogWidth + ",height=" + ModalDialogHeight + ",menubar=no,scrollbars=no";
            window.margs = vArguments;
            window.open(sURL, "_blank", sFeatures);
        }
    }
```
