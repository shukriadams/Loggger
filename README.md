# Loggger

C# logger in a single package. [Porter](https://github.com/shukriadams/porter)-ready.

## Features

- Introduces the real-world log level of "Status" messages and Verbosity as an integer scale.
- Thread safe.
- Buffered and debounced writes to disk.
- Writes to file, std out and Visual Studio debug console without requiring 10 other assemblies and 100 lines of ritual incantation.
- Performs good enough, calm down.
- Completely disregards Microsoft's standard logging interface and no one died. It's ok to have ideas of your own.

## Use 

Put log instance in your IOC system as a singleton and use wherever needed. If you want to write to multiple paths create a unique
instance for each path to avoid write collisions. 
  
    ILoggger log = new Loggger("/some/path/to/log.txt");
    log.VerbosityThreshold = 1;
    log.LogLevel = LogLevel.Warn; // default level, use this in production. 

use

    log.Status(this, "A status message at minimal verbosity, for serious auditing, not for developers. Always logged.", verbosity: 0);
    log.Status(this, "A more verbose status message, but still passes VerbosityLevel.", verbosity: 1);
    log.Status(this, "Too chatty now, won't appear unless VerbosityLevel is raised.", verbosity: 2);

    log.Debug(this, "Hey developer, this is for you, but you wont see me because LogLevel is set to warn.");
    log.Warn(this, "This is a warning and always appears, why would you want to ignore this?");

    try
    {
        throw new Exception("Testing exceptions");    
    } 
    catch(Exception ex)
    {
        log.Error(this, "This is an an error and always appears", ex);
    }

## License

MIT.
