# Loggger

C# logger in a single package. [Porter](https://github.com/shukriadams/porter)-ready.

## Features

- Completely disregards Microsoft's standard logging convention and look no one died. Maybe it's ok to be different and have ideas of your own.
- Introduces the real-world log level of "Status" messages and Verbosity as an integer scale.
- Thread safe.
- Buffered and debounced writes to disk.
- Writes to file, std out and Visual Studio debug console without requiring 10 other assemblies and 100 lines of ritual incantation.
- Performs fine enough - slower and useful is better than fast and garbage.

## Use 

Put log instance in your IOC system as a singleton and use wherever needed. If you want to write to multiple paths create a unique
instance for each path to avoid write collisions. 
  
    ILoggger log = new Loggger("/some/path/to/log.txt");
    log.VerbosityThreshold = 1;
    log.LogLevel = LogLevel.Warn; // default level, use this is in production. 

use

    log.Status(this, "I am a status message at minimal verbosity, I am here for auditing, not for developers. I can never be blocked, ever.", verbosity: 0);
    log.Status(this, "I am a more verbose status message, but I clear the VerbosityLevel.", verbosity: 1);
    log.Status(this, "I am getting too chatty now, I won't appear.", verbosity: 2);

    log.Debug(this, "Hey developer, this is for, you, but you wont see me because LogLevel is set to warn.");

    try
    {
        throw new Exception("Testing exceptions");    
    } 
    catch(Exception ex)
    {
        log.Error(this, ex);
    }

## Why? 

Log systems that run the gamut from Trace to Critical are fundamentally flawed in that they focus entirely on programmers. All log entries are either errors or developers 
making noise, and this ignores the fact that if you have an application that manages Foobars, you likely also want to write audit logs for every Foobar that you create, 
delete, buy, sell, trade etc. This information is not for programmers, it's for the grownups who run this application and want to know what is happening to their Foobars. 
"Information" is the closest we get to this concept, but Information is widely used by developers. 

## License

MIT.
