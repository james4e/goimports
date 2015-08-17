# GoImports with .goignore.

This is a fork of the original GoImports project (golang.org/x/tools/cmd/goimports), modified to respect a `.goignore` file. The location of the `.goignore` file is expected to be at the root of GOPATH. Specifically:

    $GOPATH/.goignore

### Install.

    go get github.com/eugeii/goimports

### Purpose.

The purpose of this is because in the original implementation, GoImports will traverse every directory in GOPATH, including directories which are irrelevant to Go packages. When the number of directories is large, GoImports slows significantly.

If GoImports is triggered upon saving in an editor, saving becomes extremely slow.

A good example how this may happen would be a web application directory under a Go project, that is filled with HTML/CSS/JavaScript. If there are multiple such Go projects, each with a very large web application directory, GoImports can take a long time.

Therefore, specifying those directories under `.goignore` will speed up GoImports significantly.

### The `.goignore` file.

The `.goignore` file itself is a list of *directories* to be ignored by GoImports when GoImports searches `GOPATH` for all possible Go packages. It assumes a list of directories because ignoring individual files is not very useful.

Example of `.goignore` file:

    /app
    /my-domain.com/my-project-1/large-directory-of-non-go-files
    /my-domain.com/my-project-2/another-large-directory-of-non-go-files

### How the ignores work.

For every ignore specified, GoImports will check if that ignore string is (even partially) present in the path of the package that GoImport is looking at. If it is, it completely ignores that directory. Therefore, in the example above, the following path is ignored (because there is a match against `/app`):

    /my-domain.com/webapps/app/...

### Why this is not a real fork.

The original GoImports has been moved to the official go.tools repository, which is not on GitHub. That makes it difficult to fork. In addition, this modification is really only suitable for people who want this specific `.goignore` behavior.

