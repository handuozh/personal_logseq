---
title: logseq
---

## Query commands

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
###
