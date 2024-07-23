# docusaurus-postcss-issue-repro

Two equivalent samples demonstrate the regression in Docusaurus 3.4.0.

The samples are provided for two Docusaurus versions:

  - 3.3.2 - this sample works as expected
  - 3.4.0 - this sample does not work as expected

The notable feature of the provided sample is that it uses CSS 3 syntax.
In order for this to work in Docusaurus build, the project defines a custom
Docusarus plugin 'customPostCssPlugin' in file 'docusaurus.config.ts'. The
plugin installs a PostCSS add-on that automatically translates CSS 3 syntax on
the fly during the build.


## Steps to reproduce

  1. Enter the directory of a corresponding sample:
       
       cd 3.3.2 ~or~
       cd 3.4.0 

  2. Install the missing dependencies. This creates node_modules folder not
     included in this archive:

       npm install

  3. Build the project

       npm build


## Expected outcome

  - Build finishes successfully


## Actual outcome (for Docusaurus 3.4.0)

  - [WARNING] {"file":"api/my-api.css","message":"api/my-api.css from Css Minimizer plugin\nUnexpected '}' at api/my-api.css:1:85.","compilerPath":"client"}
  - [WARNING] {"file":"api/my-api.css","message":"api/my-api.css from Css Minimizer plugin\nInvalid character(s) '}' at api/my-api.css:1:85. Ignoring.","compilerPath":"client"}
  - [WARNING] {"file":"api/my-api.css","message":"api/my-api.css from Css Minimizer plugin\nInvalid property name '& .my-api-sub-area{background' at api/my-api.css:1:40. Ignoring.","compilerPath":"client"}


## Observations and remarks

It seems that Docusaurus 3.4.0 tries to minimize CSS files from 'static' directory,
while Docusaurus 3.3.2 just kept them intact. While doing that CSS minimization,
Docusaurus 3.4.0 seems to forget to initialize PostCSS correctly, ignoring custom
plugins like they do not exist.
