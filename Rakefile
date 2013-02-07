require "bundler/gem_tasks"
require "nokogiri"
require "open-uri"
require "pp"

task :update_spec_cases do
  sources = [
    { urls: ["http://microformats.org/wiki/microformats-2"],
      html_selector: ".source-html4strict",
      json_selector: ".source-javascript",
      html_method: "inner_text"
    },
    { urls: ["http://microformat2-node.jit.su/h-adr.html"],
      html_selector: ".e-x-microformat",
      json_selector: ".language-json",
      html_method: "inner_html"
    }
  ]

  sources.each do |source|
    source[:urls].each do |url|
      document = Nokogiri::HTML(open(url).read)
      html = document.css(source[:html_selector]).map { |e| e.send(source[:html_method]) }
      json = document.css(source[:json_selector]).map { |e| e.inner_text }

      filename = url.split("/").last.gsub(/[.]\w+/, "")
      filepath = "spec/support/cases/"

      ([html.length, json.length].min).times do |index|

        File.open("#{filepath}#{filename}-#{index}.html", "w") do |f|
          f.write "<!-- #{url} -->\n"
          f.write html[index]
        end

        File.open("#{filepath}#{filename}-#{index}.js", "w") do |f|
          f.write "// #{url}\n"
          f.write json[index]
        end
      end

    end
  end
end
