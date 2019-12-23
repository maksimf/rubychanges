require 'kramdown'
require 'yaml'
require 'ostruct'

require_relative '_src/lib/util'
require_relative '_src/lib/file'
require_relative '_src/lib/toc'
require_relative '_src/lib/render'

file '_data/book.yml' => FileList['_src/*.md'] do |t|
  chapters = TOC.(t.prerequisites)
  File.write(t.name, Util.deep_stringify_keys(chapters: chapters).to_yaml)
end

rule /^\d+\.\d+\.md$/ => ->(s) { "_src/#{s}" } do |t|
  from, to = t.prerequisites.first, t.name

  puts "Rendering #{from} => #{to}"
  File.write(to, Render.(from))
end

desc 'Convert file contents from source to target (prettify)'
task contents: ('2.4'..'2.7').to_a.map(&'%s.md'.method(:%))

desc 'Render TOC for the changelog "book"'
task toc: '_data/book.yml'

task default: %i[toc contents]