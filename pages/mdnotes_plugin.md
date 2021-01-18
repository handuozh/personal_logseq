---
title: Mdnotes plugin
---

##
## Motivation
### Generate markdown based meta data and export to logseq.
### Jumps from logseq to zotero item or directly opens PDF source document.
## Mdnotes
### https://argentinaos.com/zotero-mdnotes/
### https://github.com/argenos/zotero-mdnotes
## 模板自定义示例
### 模板中需修改了local library、pdfAttachments、abstractNote等字段的md格式
zotero：编辑 > 首选项 > 高级 > 常规 > 设置编辑器：mdnotes.placeholder
![](https://raw.githubusercontent.com/xulei-shl/picgo/main/img/20210116225227.png){:height 323, :width 600}
### 修改后
###### `pdfAttachments： {"content":"PDF附件\n- {{field_contents}}", "field_contents": "{{content}}", "list_separator": "\n- "}`

###### `localLibrary：{"content":"Zotero items：{{field_contents}}"}`
###### `abstract：{"content":"[[abstract]]: \n{{field_contents}}", "field_contents": "{{content}}", "link_style": "no-links", "list_separator": ", "}`
###### `url： {"content":"{{field_contents}}", "field_contents": "{{content}}"}`

###### `title：{"content":"## {{field_contents}}", "field_contents": "{{content}}", "link_style": "no-links"}`
