---
title: logseq
---

## Change date format
### `:date-formatter "yyyy-MM-dd"`
###
```
"MMM do, yyyy"
"E, MM/dd/yyyy"
"E, yyyy/MM/dd"
"EEE, MM/dd/yyyy"
"EEE, yyyy/MM/dd"
"EEEE, MM/dd/yyyy"
"EEEE, yyyy/MM/dd"
"MM/dd/yyyy"
"MM-dd-yyyy"
"MM_dd_yyyy"
"yyyy/MM/dd"
"yyyy-MM-dd"
"yyyy_MM_dd"
"yyyy年MM月dd日"
```
## Query commands
### 1 想要查询某指定日期范围内(比如从2021-01-01到2021-02-01)的todo
#####+BEGIN_SRC clojure
#+BEGIN_QUERY
{:title " scheduled在二月的"
:query [:find (pull ?b [*])
        :where
        [?b :block/scheduled ?d]
        [?b :block/marker ?marker]
        [(>= ?d "20210201")]
        [(<= ?d "20210228")]
        [(contains? #{"NOW" "LATER" "DOING" "TODO"} ?marker)]]
:collapsed? false}
#+END_QUERY
_QUERY
RC
### 2 查询已经scheduled的所有todo项目
####
#+BEGIN_SRC 
#+BEGIN_QUERY
{:title " 已scheduled"
:query [:find (pull ?b [*])
:where
[?b :block/scheduled ?d]
[?b :block/marker ?marker2]
[(not= ?d nil)]
[(contains? #{"NOW" "LATER" "DOING" "TODO"} ?marker2)]]
:collapsed? false}
#+END_QUERY
#+END_SRC
### 3 All the tags specified in the front matter (tags: tag1, tag2)
####
#+BEGIN_SRC clojure
#+BEGIN_QUERY
{:title "All page tags"
:query [:find ?tag-name
        :where
        [?tag :tag/name ?tag-name]]
:view (fn [tags]
        [:div
         (for [tag (flatten tags)]
           [:a.tag.mr-1 {:href (str "/page/" tag)}
            (str "#" tag)])])}
#+END_QUERY
#+END_SRC
### 3
## 插件需求
### {{embed ((6016b329-b18a-41eb-b303-a5e9bfc1602f)) }}
### 自动把所有日期,md/org格式转换成设置里面的格式
### IFFFT快捷工作流,整合其他软件
### 日历Calendar,提醒等
##
##
##
