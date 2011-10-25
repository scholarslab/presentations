# Scholars' Lab Presentations

Slides written in Markdown and formatted/organized for [Showoff](https://github.com/schacon/showoff).


# Dependencies

In order to generate the PDFs, ```rmagick``` and ```pdfkit``` gems are
needed. The ```pdfkit``` gem has a dependency on the WKHTMLTOPDF library
to convert the HTML to PDF for that output. Fortunately, this is
available through brew. These two commands should get you going:

    brew install WKHTMLTOPDF
    bundle
