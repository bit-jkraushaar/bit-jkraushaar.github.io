---
layout: post
title:  "Git Repository ohne Versionsinformationen kopieren"
tags: Tools Git
---

Von Zeit zu Zeit möchte man ein Git Repository ohne Versionsinformationen kopieren.
Wie das mit zwei simplen Befehlen geht, beschreibe ich in diesem Artikel.

Gefunden habe ich diese Lösung auf [Stack Overflow](https://stackoverflow.com/questions/160608/do-a-git-export-like-svn-export).

```
git archive master | tar -x -C /somewhere/else
```

Zunächst wird der `master` Branch als `tar` archiviert.
Über die Pipe wird das Archiv an `tar` weitergegeben und wieder entpackt.
Zuvor wechselt `tar` allerdings noch in das angegebene Verzeichnis (durch `-C`).
Letztendlich wird der `master` Branch des aktuellen Repositories dadurch nach `/somewhere/else` kopiert.
