# WebDirect Font Utility
 WebDirect Font Utility is an HTML-embedded JavaScript utility that leverages the [TypeKit Web Font Loader](https://github.com/typekit/webfontloader) from Adobe to inject custom fonts into FileMaker WebDirect sessions.

 ## Introduction
 Out of the box, FileMaker Server does not support an extensive variety of fonts that can be used when deploying a solution for WebDirect, and there are no available options for including fonts in your solution's file.
 
 Even if a font is installed on the local machine, it still may not appear correctly in FileMaker WebDirect. The WebDirect Font Utility can be used to include custom fonts that work inside of FileMaker WebDirect sessions. This is done by providing the fonts to WebDirect in a "web-friendly," standards-compliant way: CSS.

 ## Prerequisites
 To make use of the utility, you'll need to have your typefaces in a prepared format. Most typefaces ship as `.otf` or `.ttf` files, but neither of these are compatible in the modern browser. However, they can easily be converted and prepared using the online utility at [font-converter.net](https://www.font-converter.net/).

 Using that awesome little tool, you'll receive a `.zip` bundle with everything you need, including the converted typeface files for all browsers *(yes, including IE — the thorn in my side)* as well as the `.css` file you need to declare the fonts to the WebDirect Font Utility.

## Font Hosting
Before the utility can be used, the fonts you intend to use need to be uploaded to somewhere that the browser can access. This could be somewhere like Dropbox or a custom web server, but my preferred method is to put them on the FileMaker Server host in the `htdocs` directory.

On the FileMaker Server host system, copy the contents of the `.zip` file you received from [font-converter.net](https://www.font-converter.net/) to the directory that applies to your installation:
*  **macOS**: `/Library/FileMaker Server/HTTPServer/htdocs/httpsRoot`
*  **Windows**: `%PROGRAMFILES%\FileMaker\FileMaker Server\HTTPServer\conf`
   > It's super-important to ensure that you put the files into the **httpsRoot** and not the insecure root. WebDirect is served over HTTPS, so we need that to match.

   > If you anticipate using this utility in more than one solution, you may want to consider adopting an organized structure for your fonts in this directory. So long as you keep special chars and whitespace out of the folder names, you are free to organize as much or little as you'd like.

 ## Install
 To install and make use of the WebDirect Font Utility, you'll need to define a web viewer on the FileMaker layout where you want to use the custom fonts. Inside of that web viewer's URL property, copy and paste the contents of [dist/fontutility-datauri.fmcalc.txt](https://github.com/stephancasas/webdirect-font-utility/blob/master/dist/fontutility-datauri.fmcalc.txt).

 > Why are we using a data URI?
 > * Unless we want to host the utility ourselves via the FileMaker Server host's HTTP server, we need to include the utility as a data URI to prevent the browser from throwing a "same origin" exception. It also makes swapping CSS sources super-easy, because we can easily change our global variable to specify a new target (more on that later).

 ## Usage
 After completing the installation above, using the utility is just a matter of providing it with the URL of your fonts' CSS. So long as you didn't change the content of the data URI calculation, this is passed using the global variable `$$webd_font_util_css`.
 
 If you've followed the instructions under the "Font Hosting" section of the README, the URL for your fonts' CSS should look something like this:

 ```plaintext
 https://filemakerserver.example.com/style.css
 ```

 In a script of your choice, populate the `$$webd_font_util_css` variable with the font CSS URL as expressed above, and then trigger the script in such a way that it will be called at application load or layout entry.

 That's it — fonts should now appear consistently in both WebDirect and FileMaker Pro. If you encounter a problem, please raise an issue here on GitHub.

 ## License
 [MIT](http://opensource.org/licenses/MIT) -- *Hell yeah! Free software!*