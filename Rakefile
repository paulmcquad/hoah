namespace :book do
    desc 'build basic book formats'
    task :build do
  
      begin
        # Find out the Version Number and Assign
        version_string = `git describe --tags`
        if version_string.empty?
          version_string = '0'
        end

        # Add a Version Number and Date-stamp to the book (PDF,HTML)
        date_string = Time.now.strftime("%Y-%m-%d")

        # Add them to the params label
        params = "--attribute revnumber='#{version_string}' --attribute revdate='#{date_string}'"
        
        puts "Generating Developers list"
        # Find the developers, exclude me, cut to remove sections and space out the names,
        # column to create a width, output to Developers.txt
        `git shortlog -s | grep -v -E "(McQuade)" | cut -f 2- | column -c 120 > book/Preface/Developers.txt` #
  
        # Run Asciidoctor to build the book
        puts "Converting to HTML..."
        `bundle exec asciidoctor #{params} -a data-uri hoah.adoc`
        puts " -- HTML output at hoah.html"
  
        puts "Converting to EPub..."
        `bundle exec asciidoctor-epub3 #{params} hoah.adoc`
        puts " -- Epub output at hoah.epub"
  
       # puts "Converting to Mobi (kf8)..."
       # `bundle exec asciidoctor-epub3 #{params} -a ebook-format=kf8 hoah.adoc`
       # puts " -- Mobi output at hoah.mobi"
  
        puts "Converting to PDF... (this one takes a while)"
        `bundle exec asciidoctor-pdf #{params} hoah.adoc 2>/dev/null`
        puts " -- PDF output at hoah.pdf"
  
      end
    end
  end
  
  task :default => "book:build"
  
