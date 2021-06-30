---
title: org-mode
---

- Headlines
  heading:: true
    - `Ctrl-c`, `Ctrl-w`: Move current head to another place
    - `Alt-up/down`: cursor navigate current head up / down
    - `Ctrl-ALt-up/down`: move current head up / down
    - `Alt-left/right`: change to parent/child
    - `SPC-m-h`: toggle heading
    - `SPC-m-i`: toggle item
    -
- Todo keywords
  heading:: true
    - Shorcut
        - Mark with `SPC-m-t-t`
        - Toggle with `Return`
    - Keywords
        - -TODO (t)
          todo:: 1608515146262
        - PROJ (p)
        - STRT (s)
        - -WAIT (w)
          wait:: 1608515175131
        - HOLD (h)
        - -DONE (d)
          done:: 1608515185964
        - KILL (k)
    - Progress
        - Add `[/]` at the head before sub lists.
    - Tick/Untick
        - `SPC-m-x` to toggle
        - `Ctr-C` `Ctr-c` alternative (native Emacs)
- Latex Support
  heading:: true
    -
      ```org
      \begin{equation}
      x=\sqrt{b}
      \end{equation}
      ```
    - For inline, close with `$` or `\( \)`
    - `Ctrl-c` `Ctrl-x` `Ctrl-l`: Preview current latex equation (cursor location)
- Agenda
  heading:: true
    - `SPC-o` `A`: agenda menu
    - `SPC-m-d`: org-schedule