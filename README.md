# Font Awesome Cheat C#
This repo contains code that can be used to generate Font Awesome unicodes for your c# projects like WPF, Xamarin and ASP

I have this need to use FA across my Xamarin and ASP apps which would save me from downloading images for every little thing. 
I found a site online that extracted codes from `https://fontawesome.com/cheatsheet` directly using Javascript. I tried it but it appears the site has changed. 

I quickly decided to create mine using a C# .Net Console Application and the HTMLAgility Nuget package.
Steps to recreate:
* Visit `https://fontawesome.com/cheatsheet`
* Inspect the source, locate the body, expand it, locate ```<div class="ph4 ph6-ns pv6 ph0-pr pv0-pr bg-white">``` minimize the it, right click, click copy.
* Add an html file to your project and set build to 'copy always' and paste the code into the file.

Browse the source to see the full codes.

            private static void Main(string[] args)
        {
            //Goto https://fontawesome.com/cheatsheet 
            //Inspect  the source, locate the body, expand it, locate <div class="ph4 ph6-ns pv6 ph0-pr pv0-pr bg-white"> minimize the it, right click, copy.
            //Add an html file to your project and set build to 'copy always' and paste the code into the file.
            string path = @"test.html";

            //Initializes the Variable to use
            HtmlDocument htmlDoc = new HtmlDocument();
            //load html into variable
            htmlDoc.Load(path);

            //Load the three sessions (solid, regular, brands ) into the var 
            HtmlNodeCollection htmlSessions = htmlDoc.DocumentNode.SelectNodes("//section");
            //Iterate through sessions to process.
            Console.WriteLine($@"public class FontAwesome");
            Console.WriteLine(@"{");
            foreach (HtmlNode session in htmlSessions)
            {
                Console.WriteLine($@"public static class {Processor.Edit(session.Id)}");
                Console.WriteLine(@"{");
                HtmlNodeCollection session_articles = session.SelectNodes("//article");
                foreach (HtmlNode article in session_articles)
                {

                    string title = article.Id;
                    HtmlNode dlNode = article.ChildNodes[1];
                    HtmlNode ddNode = dlNode.ChildNodes[5];
                    string unicode = ddNode.InnerText;
                    string output = $@" public static string {Processor.Edit(title)} = ""\u{unicode}"";"; //&#x
                    Console.WriteLine(output);
                }
                Console.WriteLine("}");
                Console.WriteLine("");
                Console.WriteLine("");
                Console.WriteLine("");
            }
            Console.WriteLine("}");
        }
