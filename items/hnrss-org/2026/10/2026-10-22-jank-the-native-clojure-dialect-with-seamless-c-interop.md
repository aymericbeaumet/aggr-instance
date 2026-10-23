---
title: Jank – The native Clojure dialect with seamless C++ interop
link: https://jank-lang.org/
source: hnrss-org
published: 2026-10-22T23:56:36Z
updated: 2026-10-22T23:56:36Z
first_seen: 2026-10-23T09:31:36.349306873Z
authors:
- giancarlostoro
content: extracted
html: 2026-10-22-jank-the-native-clojure-dialect-with-seamless-c-interop.html
remote_preview:
  url: https://jank-lang.org/img/logo-text-dark.png
---

**jank is Clojure on top of C**++. It has C++ interop unlike any other language. Templates, virtual functions, exceptions, overload resolution, and RAII all just work in jank. No boilerplate necessary. Just include your headers and go!

**jank is a robust Clojure dialect**. It offers interactive REPL-driven development powered by a Clang-based C++ JIT compiler.

```
(ns my.app.filesystem
  (:include "boost/filesystem.hpp")
  (:refer-global :rename {boost.filesystem.file_size file-size}))

(defn file-info [file-path]
  (try
    (let [bytes (file-size (cpp/cast std.string file-path))]
      {:path file-path
       :size bytes})
    (catch boost.filesystem.filesystem_error e
      {:path file-path
       :error (.what e)})))

(file-info "/etc/passwd")
; {:path "/etc/passwd", :size 4025}

(file-info "/root/.bash_history")
; {:path "/root/.bash_history"
;  :error "boost::filesystem::file_size: Permission denied [system:13]: \"/root/.bash_history\""}
```

jank is a full-on Clojure dialect, with C++ interop instead of Java interop. There's a `:jank` reader conditional for when you need it.

jank can include any C++ source, link to any C or C++ library, and it uses the C++ type system. jank even compiles to C++!

jank uses Leiningen-based project management and has a rich native build system. It's easy to add C and C++ dependencies to your project.

Use your favorite nREPL editor plugin. jank contains an LLVM-based JIT runtime to compile C++ code on the fly.

### Easy project management

jank's native build system can find your installed native dependencies or build them from source. Adding a new dependency is a one line change.

```
(defproject imgui+glfw "0.1-SNAPSHOT"
  :dependencies [[org.jank-lang.commons/imgui-glfw-sys "2026.06-6"]
                 [org.jank-lang.commons/imgui-opengl2-sys "2026.06-6"]
                 [org.jank-lang.commons/gl-sys "2026.06-1"]]
  :plugins [[org.jank-lang/lein-jank "2026.06-2"]]
  :middleware [leiningen.jank/middleware]
  :main imgui+glfw.main
  :profiles {:base {:jank {:target-dir "target/debug"
                           :optimization-level 0}}
             :release {:jank {:target-dir "target/release"
                              :optimization-level 3}}})
```

### jank is great for game dev

jank brings functional, immutable data and REPL-driven development to your game development workflow. With jank, you can reach into C++ whenever you want the extra performance. No bridges, no function/type registration, no marshalling, no callbacks.

```
(ns imgui+glfw.main
  (:include "GLFW/glfw3.h"
            "imgui.h" "imgui_impl_glfw.h" "imgui_impl_opengl2.h"))

(defn -main []
  (cpp/glfwInit)

  (let [window* (cpp/glfwCreateWindow 400 300
                                      "jank"
                                      cpp/nullptr cpp/nullptr)]
    (cpp/glfwMakeContextCurrent window*)
    (cpp/ImGui.CreateContext)
    (cpp/ImGui_ImplGlfw_InitForOpenGL window* true)
    (cpp/ImGui_ImplOpenGL2_Init)

    (while (cpp/! (cpp/glfwWindowShouldClose window*))
      (cpp/glfwPollEvents)
      (cpp/ImGui_ImplOpenGL2_NewFrame)
      (cpp/ImGui_ImplGlfw_NewFrame)
      (cpp/ImGui.NewFrame)

      (when (cpp/ImGui.Begin "jank")
        (cpp/ImGui.Text "Hello, jank and imgui!"))
      (cpp/ImGui.End)

      (cpp/ImGui.Render)
      (cpp/ImGui_ImplOpenGL2_RenderDrawData (cpp/ImGui.GetDrawData))
      (cpp/glfwSwapBuffers window*))

    (cpp/glfwTerminate)))
```

![](https://jank-lang.org/img/landing/lein-compile.png)
