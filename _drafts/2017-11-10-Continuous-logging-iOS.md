---
layout: single
title:  "Continous logging to text file in swift"
date:   2017-11-10 16:16:01 +0100
categories: coding
tags: ios apple swift
---

I consider iOS develppment much more user and beginner friendly than Android development. But despite that, there are some things that simply drive me crazy. Not being able to play with the files themselves is one thing.

Whereas on Android, I can just create files wihin apps and then copy them to the PC, it took me three hours to fuigure out how to do such a simple task in iOS. Spoiler alert, you CAN'T! Yuou either have to send the file to you through airdrop or similar proprietary bullshit, or email. Or, you have to expose these files through iTunes. All it extremely convoluted if you ask me :(

My task was quite simple. For research purposes I needed to log certain phone metrics about twenty times per second. I didn't want to log it into arrays, because that would become quite large and in case of failure, the entire array would be lost. I needed to log it failsafe every time intpo a file and in case of failure be able to retrieve at least some part.

# Options
Googling provided many information, that were quite difficult to parse. The simplest option of `String.write(to: URL)` was supposedly synchronous and because my loggging events were fired quite irreguraly and I couldn't afford any mistakes, I needed asynchronous stream writing. 


# Basic implementation
First we need to get a file to write to. You can simply create your file with the following code
```swift
let filename = "interesting-file"
let DocumentDirURL = try! FileManager.default.url(for: .documentDirectory, in: .userDomainMask, appropriateFor: nil, create: true)
let logURL = DocumentDirURL.appendingPathComponent(filename).appendingPathExtension("txt")
//creates the file
FileManager.default.createFile(atPath: logURL.path, contents: nil, attributes: nil)
```
I am unsure if you need to create the file before opening the stream to it, but I had some troubles withouth creating it first.

One of the first mistakes I did was I was trying to create and write to a file at the wrong path! Because output stream takes path to a file, I thought I shoudl send him `URL.absolutePath`, but that stream did not open. As soon as I changed it to `URL.path` everything was fine. Same applies to the `OutputStream`.

```swift
//DOENS'T WORK
FileManager.default.createFile(atPath: logURL.absolutePath, contents: nil, attributes: nil)
//WORKS
FileManager.default.createFile(atPath: logURL.path, contents: nil, attributes: nil)
```

Before Swift 4, you needed to write your [own function](https://stackoverflow.com/questions/26989493/how-to-open-file-and-append-a-string-in-it-swift) to write `String` to OutputStream, but it is simpler now. So you can just call write with your encoded text.

``` swift
let logFile = OutputStream(toFileAtPath: logURL.path, append: true)
logFile?.open()
let text = "Something interesting to write in \n"
let encodedtext = [UInt8](text.utf8)
let bytesWritten = logFile.write(encodedtext, maxLength: text.count)
if bytesWritten < 0 {
    print("Couldn't write into the file")
}
logFile?.close()
```

# Putting it in a class
```swift
import Foundation

class Logger {
    
    let ENCODING = String.Encoding.utf8.rawValue
    let ALLOW_LOSSY_CONVERSION = false
    let logPath : URL
    var logFile : OutputStream?
    
    init(_ filename : String) {
        logPath = Logger.createFilePath(filename)
        createLogFile()
    }
    
    func openLogFile(){
        logFile = OutputStream(toFileAtPath: logPath.path, append: true)
        logFile?.open()
    }
    
    func closeLogFile(){
        logFile?.close()
    }
    
    func write(_ text: String){
        let text = "\(string)\n"
        let encodedtext = [UInt8](text.utf8)
        let bytesWritten =logFile?.write(encodedtext, maxLength: text.count)
        if bytesWritten < 0 {print("failure to write")}
    }
    
    static func createFilePath(_ filename: String) -> URL {
        let DocumentDirURL = try! FileManager.default.url(for: .documentDirectory, in: .userDomainMask, appropriateFor: nil, create: true)
        let fileURL = DocumentDirURL.appendingPathComponent(filename).appendingPathExtension("csv")
        return fileURL
    }
    
    //creates a file at given log path
    func createLogFile() -> Bool{
        return FileManager.default.createFile(atPath: logPath.path, contents: nil, attributes: nil)
    }
    
    //Gets all files we created in an optional URL array
    static func listFiles() -> [URL]?{
        let docsURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!
        let content = try? FileManager.default.contentsOfDirectory(at: docsURL, includingPropertiesForKeys: nil, options: [])
        let csvFiles = content?.filter{ $0.pathExtension == "csv" }
        return csvFiles
    }
```

You can also create an extension of the OutputStream, to clean everything up a bit. Rede

```swift
extension OutputStream {
    func writeString(_ string: String){
        let encodedString = [UInt8](string.utf8)
        let bytesWritten = write(encodedString, maxLength: text.count)
        if bytesWritten < 0 {print("failure to write")}
    }
}
```
You can then change the `Logger` function to 
```swift
class Logger {
    func writeLine(_ text: String){
        let text = "\(string)\n"
        writeString(text)
    }
    func writeString(_ text: String){
       logFile?.writeString(text)
    }
}
```

# Testing

If you plan to write to it often, just keep it open and write in as you feel like it. I tested how fast can you do that with a simple timer function

```swift
class LoggingTest {
    var isLogging : Bool = false
    var testTimer : Timer?
    var loggedLines : Int = 0
    var loggingSpeed : Double = 0.1
    var logger : Logger

    func init(){
        logger = Logger("test-speed")
    }

    func startLogging(){
        logger?.openFile()
        isLogging = true
        testTimer = Timer.scheduledTimer(timeInterval: TimeInterval(loggingSpeed), target: self, selector: (#selector(LoggingTest.logTestLine)), userInfo: nil, repeats: true)
    }

    func stopLogging(){
        isLogging = false
        testTimer?.invalidate()
        testTimer = nil
        loggedLines = 0
        logger?.closeFile()
    }

    @objc func logTestLine(){
        loggedLines += 1
        logger?.writeLine("writing line number \(String(loggedLines))")
    }
}
```


With this implementation, I can log more than 50 lines per second. 