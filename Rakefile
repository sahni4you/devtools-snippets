require 'nokogiri'

# <div class="snippet" data-src="snippets/jquerify.js"></div>

task :deploy do
    system "git checkout gh-pages"
    #system "git merge master"
    #system "git push origin gh-pages"

    Rake::Task["build"].execute

    system "git checkout master"
end

task :build do
  FileUtils.cd(File.dirname(__FILE__))
  replace_snippets('snippets.html', 'index.html')
end

def replace_snippets(infile, outfile)

    doc = Nokogiri::HTML( File.read(infile) )
    doc.css("div.snippet").each do |div|
        cmd = "pygmentize -f html " + div.attr("data-src")
        content = `#{cmd}`
        div.inner_html = content
    end

    renderedhtml = doc.to_html()

    File.open(outfile, "w") do |io|
        io.write renderedhtml
    end
end

task :default => [:build]
