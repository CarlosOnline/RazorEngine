RazorEngine
===========

C# Razor Engine compiler for non web programs. 
Based off of work by Antaris.
My version adds some niceness around processing the errors, nothing else.

Antaris links
-------------
     http://razorengine.codeplex.com/
     http://github.com/Antaris/RazorEngine

```
using System;
using System.Collections.Generic;
using System.IO;
using System.Text;

namespace RazorEngine
{
    public class Example
    {
        public class Model
        {
            public string Name = "Example";
            public string Table = "ExampleTable";

            public List<string> Columns = new List<string>
            {
                "First",
                "Second",
                "Third",
                "Fourth",
            };
        }

        public static int Main(string[] args)
        {
            try
            {
                // Pass template file as first argument, defaults to Example.Template.txt
                var filePath = args.Length > 0 ? args[0] : Path.Combine(System.Reflection.Assembly.GetExecutingAssembly().Location, "Example.Template.txt");
                var fullFilePath = Path.GetFullPath(filePath);
                if (!File.Exists(fullFilePath))
                    throw new Exception(string.Format("Missing template file {0}", fullFilePath));

                // Create model and compile with Razor
                var model = new Model();
                var template = File.ReadAllText(fullFilePath);
                var result = Razor.ParseWithErrors(template, model);

                // Output results to Output.txt
                var folder = Path.GetDirectoryName(fullFilePath);
                var outputFile = Path.Combine(folder, "Output.txt");
                File.WriteAllText(outputFile, result, Encoding.ASCII);
                Console.WriteLine("Generated {0}", outputFile);

                Console.WriteLine("Successfully ran example");
            }
            catch (Exception ex)
            {
                Console.WriteLine("Example error: {0}", ex);
                return 1;
            }
            return 0;
        }
    }
}

```
