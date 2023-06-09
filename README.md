# ShellToolKit
<p align="center">
<a href="https://github.com/Kitura/ShellToolKit/actions?query=workflow%3ASwift"><img src="https://github.com/Kitura/ShellToolKit/workflows/Swift/badge.svg" alt="build status"></a>
<img src="https://img.shields.io/badge/os-macOS-green.svg?style=flat" alt="macOS">
<img src="https://img.shields.io/badge/os-linux-green.svg?style=flat" alt="Linux">
<a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache2-blue.svg?style=flat" alt="Apache 2"></a>
<br/>
<a href="https://swiftpackageindex.com/Kitura/ShellToolKit"><img src="https://img.shields.io/endpoint?url=https%3A%2F%2Fswiftpackageindex.com%2Fapi%2Fpackages%2FKitura%2F ShellToolKit%2Fbadge%3Ftype%3Dswift-versions"></a>
<a href="https://swiftpackageindex.com/Kitura/ShellToolKit"><img src="https://img.shields.io/endpoint?url=https%3A%2F%2Fswiftpackageindex.com%2Fapi%2Fpackages%2FKitura%2F ShellToolKit%2Fbadge%3Ftype%3Dplatforms"></a>
</p>
`ShellToolKit` is a set of classes that are typically useful when using Swift as a command-line tool.

## Installation
### [Swift Package Manager](https://swift.org/package-manager/)

```swift
import PackageDescription

let package = Package(
    name: "YourAwesomeSoftware",
    dependencies: [
        .package(url: "https://github.com/Kitura/ShellToolKit.git", from: "0.1.0")
    ],
    targets: [
        .target(
            name: "MyApp",
            dependencies: ["ShellToolKit"]
        )
    ]
)
```

### [swift-sh](https://github.com/mxcl/swift-sh)

```
#!/usr/bin/swift sh

import ShellToolKit          // @Kitura


```

## System Action

Currently the only class supported is `SystemAction` which allows command-line tools to more easily support variations of verbose/dry-run.  It uses the [Rainbow](https://github.com/onevcat/Rainbow) library for colorized output and [SwiftShell](https://github.com/kareman/SwiftShell) for executing commands.

 A typical swift-argument-parser based program may look something like this:
 
 ```swift
 
 import ArgumentParser   // https://github.com/apple/swift-argument-parser.git

struct BuildCommand: ParsableCommand {
    @Flag(name: .shortAndLong, help: "Enable verbose mode")
    var verbose: Bool = false

    @Flag(name: [.customLong("dry-run"), .customShort("n")], help: "Dry-run (print but do not execute commands)")
    var enableDryRun: Bool = false


    mutating func run() throws {
        let actions: SystemAction
        
        if enableDryRun {
            actions = CompositeAction([SystemActionPrint()])
        } else if verbose {
            actions = CompositeAction([SystemActionPrint(), SystemActionReal()])
        } else {
            actions = CompositeAction([SystemActionReal()])
        }
     
        try actions.runAndPrint(command: "echo", "Hello", "World!")
    }
 
 }
 ```


##### References

## Capturing stdin/stdout
* [How to run an external program using Process](https://www.hackingwithswift.com/example-code/system/how-to-run-an-external-program-using-process)
* [StackOverflow: Make pipe fd return true for is_atty() in child process](https://stackoverflow.com/questions/58794526/make-pipe-fd-return-true-for-is-atty-in-child-process)
* [Streams of Cocoa: Why It's Still Worth Knowing NSStream](https://pspdfkit.com/blog/2021/streams-of-cocoa-why-its-still-worth-knowing-nsstream/)
* [The very basics of a terminal emulator](https://www.uninformativ.de/blog/postings/2018-02-24/0/POSTING-en.html)
* [StackOverflow: How to redirect stdout to stdin using fork and pipe and execvp in c?](https://stackoverflow.com/questions/49407406/how-to-redirect-stdout-to-stdin-using-fork-and-pipe-and-execvp-in-c)
* [Foundation: NSStream.getBoundStreamsWithBufferSize:inputStream:outputStream:](https://developer.apple.com/documentation/foundation/nsstream/1412683-getboundstreamswithbuffersize?language=objc)
* [Streams of Cocoa: Why It's Still Worth Knowing NSStream](https://pspdfkit.com/blog/2021/streams-of-cocoa-why-its-still-worth-knowing-nsstream/)
* [PtyKit](https://github.com/Kaiede/PtyKit)
* [SwiftyPOSIX](https://github.com/bscothern/SwiftyPOSIX)

### Related/interesting projects
* [SwiftTermApp](https://github.com/migueldeicaza/SwiftTermApp)
* [SwiftTerm](https://github.com/migueldeicaza/SwiftTerm)
* [Curassow](https://github.com/kylef-archive/Curassow/blob/269b9bca201b8690653d76d69c1542f8e304b394/Sources/Curassow/Socket.swift)

