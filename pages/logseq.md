---
title: logseq
---

## Query commands
:PROPERTIES:
:heading: true
:END:
###
#+BEGIN_SRC 
+BEGIN_QUERY
{:title "📅 过期DeadLine"
:query [:find (pull ?b [*])
:in $ ?start
:where
[?b :block/deadline ?d]
[?b :block/marker ?marker]
[(< ?d ?start)]
[(contains? #{"NOW" "LATER" "DOING" "TODO"} ?marker)]]
:inputs [:today]
:collapsed? false}
#+END_QUERY
#+END_SRC
###
#+BEGIN_SRC 
:title "📅 临近DeadLine"
:query [:find (pull ?b [*])
:in $ ?start ?next
:where
[?b :block/deadline ?d]
[?b :block/marker ?marker]
[(> ?d ?start)]
[(< ?d ?next)]
[(contains? #{"NOW" "LATER" "DOING" "TODO"} ?marker)]]
:inputs [:today :7d-after]
:collapsed? false}
#+END_SRC
### 查询所有包含特定词的标题
####
#+BEGIN_SRC 
#+BEGIN_QUERY
{:title [:code "Pages that have \"clojure\" inside"]
 :query [:find (pull ?p [*])
         :where [?p :page/name ?name]
         [(clojure.string/includes? ?name "clojure")]]}
#+END_QUERY
#+END_SRC
## CSS tips
###
#+BEGIN_SRC 
/*Supporter colors:
https://github.com/mrmrs/colors
.navy { color: #001F3F; }
.blue { color: #0074D9; }
.aqua { color: #7FDBFF; }
.teal { color: #39CCCC; }
.olive { color: #3D9970; }
.green { color: #2ECC40; }
.lime { color: #01FF70; }
.yellow { color: #FFDC00; }
.orange { color: #FF851B; }
.red { color: #FF4136; }
.fuchsia { color: #F012BE; }
.purple { color: #B10DC9; }
.maroon { color: #85144B; }
.silver { color: #DDDDDD; }
.gray { color: #AAAAAA; }
.black { color: #111111; }
*/
.tag[href*="/c-"] {
     display:none !important;  
}

.tag[href*="/c-"] + mark, 
.tag[href*="/c-"] + .page-reference > .page-ref {
    color: white !important;
 border-radius: 5px;
 padding-left: 5px;
 padding-right: 5px;
 font-weight: bold;
}

div[data-refs-self*="c-red"] > div:first-of-type .block-content {
    background-color: red !important;
}
#+END_SRC
###
