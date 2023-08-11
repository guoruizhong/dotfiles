options(repos = c(CRAN = "https://cloud.r-project.org/"))
# options(repos = c(CRAN = "https://mirrors.pku.edu.cn/CRAN/"))
# options(repos = c(CRAN = "https://mirrors.upm.edu.my/CRAN/"))
# .libPaths("~/R/library")
# message("Using library: ", .libPaths()[1])


options(error = function() {
  calls <- sys.calls()
  if (length(calls) >= 2L) {
    sink(stderr())
    on.exit(sink(NULL))
    cat("Backtrace:\n")
    calls <- rev(calls[-length(calls)])
    for (i in seq_along(calls)) {
      cat(i, ": ", deparse(calls[[i]], nlines = 1L), "\n", sep = "")
    }
  }
  if (!interactive()) {
    message("Exiting on error")
    q(status = 1)
  }
})

options(languageserver.lint_cache = TRUE)

options(languageserver.formatting_style = function(options) {
  styler::tidyverse_style(
    scope = "indention",
    indent_by = options$tabSize
  )
})

# options(datatable.quiet = TRUE,
#   datatable.print.class = TRUE,
#   datatable.print.keys = TRUE)
# options(future.rng.onMisuse = "ignore")

if (interactive() && Sys.getenv("RSTUDIO") == "") {
  Sys.setenv(TERM_PROGRAM = "vscode")
  source(file.path(Sys.getenv(if (.Platform$OS.type == "windows") "USERPROFILE" else "HOME"), ".vscode-R", "init.R"))
}

### t disables the original simple plot watcher provided by vscode-R session watcher 
### (which replays user graphics into a png file) and use httpgd as the graphics device 
### and opens a new WebView tab in VSCode to show the graphics.
if (interactive() && Sys.getenv("TERM_PROGRAM") == "vscode") {
  if ("httpgd" %in% .packages(all.available = TRUE)) {
    options(vsc.plot = FALSE)
    options(device = function(...) {
      httpgd::hgd(silent = TRUE)
      .vsc.browser(httpgd::hgd_url(), viewer = "Beside")
    })
  }
}