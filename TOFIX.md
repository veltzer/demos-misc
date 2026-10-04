# TOFIX

Findings from a code scan on 2026-10-04.

## Medium

- `grails/grails-intro/target/stacktrace.log:1` - 11 files of Grails build output (`target/test-reports/**`, `target/stacktrace.log`) are committed, plus a top-level `grails/grails-intro/stacktrace.log`; `rsconstruct.toml:26,48,54` carries "not linted: ... committed grails build output" comments to work around them. Delete the build output, ignore `target/`, and drop those comments.
- `llvm/Makefile:1` - uses `llvm-gcc`, which was removed from LLVM in 3.0 (2011) and is not installable on current distros, so every target fails; `hello.bc` (line 12) also uses `-c` without `-emit-llvm`, so even with a compiler it would produce a native object, not bitcode. Switch to `clang` (`clang -emit-llvm -c` for `.bc`).
- `gwt/doc/install_gwt.txt:2` - setup instructions depend on the Google Plugin for Eclipse update site `http://dl.google.com/eclipse/plugin/4.2`, which is discontinued (the referenced docs page now redirects to `cloud.google.com/eclipse/docs/migrating-gpe`); the GWT examples also ship no build file (no `pom.xml`/`build.xml`/`.classpath`), so there is no documented way to build them today. Document a current toolchain (e.g. the GWT Maven plugin) or mark the material as historical.
- `README.md:1` - the README is the old all-in-one "Demos" monorepo text ("samples for many programming languages", "Mark Veltzer, 2011-2020") in setext style; it does not say what this repo actually contains (gwt, grails, m4, gpp, llvm) or how to build any of it. Rewrite it for this repo.

## Low

- `grails/grails-intro/.hg_archival.txt:1` - Mercurial leftovers (`.hg_archival.txt`, `.hgignore`) and IntelliJ project files (`shareFile1.iml`, `shareFile1-grailsPlugins.iml`) are committed; delete them.
- `grails/grails-intro/grails-app/conf/BootStrap.groovy:10` - creates `admin`/`admin` and `user`/`user` accounts unconditionally, i.e. also in the `production` environment (`DataSource.groovy:29`); wrap the seeding in an `Environment.DEVELOPMENT` check.
- `m4/TODO.txt:2` - says "Add Makefile to build them and to clean them", but `m4/Makefile` already exists; drop the stale item. The `.out` files `m4/Makefile` and `gpp/Makefile` generate are not ignored by any `.gitignore`, and `gpp/Makefile` has no `clean` target (unlike `m4/Makefile:9`).
- `doc/TODO.txt:1` - items refer to "the makefile"/"all demos" of the former monorepo build, none of which exists here; delete or rewrite.
- `gwt/scripts/eclipse_gwt.sh:5` - hard-codes `~/install/eclipse-gwt` and `~/workspace-gwt` and discards all output, so a missing install fails silently in the background; check the path exists and report an error.
