---
title: logseq
---

- Query commands
  heading:: true
    -
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
    -
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
    - 查询所有包含特定词的标题
        -
          #+BEGIN_SRC 
          #+BEGIN_QUERY
          {:title [:code "Pages that have \"clojure\" inside"]
           :query [:find (pull ?p [*])
                 :where [?p :page/name ?name]
                 [(clojure.string/includes? ?name "clojure")]]}
          #+END_QUERY
          #+END_SRC
        -
          #+BEGIN_SRC
          #+BEGIN_QUERY
          {:title "包含tag1和tag2的page"
           :query [:find (pull ?p [*])
                 :in $ ?t1 ?t2
                 :where
                 [?b :block/page ?p]
                 [?c :block/page ?p]
                 [?b :block/ref-pages ?rp]
                 [?c :block/ref-pages ?rp2]
                 [?rp :page/name ?t1]
                 [?rp2 :page/name ?t2]]
           :inputs [ "tag1" "tag2"]}
          #+END_QUERY
          #+END_SRC
    - 查询所有schedule的同时包含某个tag的block
        -
          #+BEGIN_SRC 
          #+BEGIN_QUERY
          {:title " 已scheduled"
          :query [:find (pull ?b [*])
          :where
          [?b :block/scheduled ?d]
          [?b :block/marker ?marker2]
          [?p :page/name "related"]
          [?b :block/ref-pages ?p]
          [(not= ?d nil)]
          [(contains? #{"NOW" "LATER" "DOING" "TODO"} ?marker2)]]
          :collapsed? false}
          #+END_QUERY
          #+END_SRC
- CSS tips
    -
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
    -
      #+BEGIN_QUERY
      {:title [:code "Pages that have \"epipolar\" inside"]
       :query [:find (pull ?p [*])
             :where [?p :page/name ?name]
             [(clojure.string/includes? ?name "epipolar")]]}
      #+END_QUERY