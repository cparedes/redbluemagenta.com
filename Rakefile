desc 'Run all tasks'
task :default => [:tags, :commit]

desc 'Commit all files'
task :commit do
	sh "git commit -am 'Committing files' ; git push"
end

desc 'Generate tags page'
task :tags do
  puts "Generating tags..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters
  
  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  site.categories.sort.each do |category, posts|
    html = ''
    html << <<-HTML
---
layout: default
title: Postings filed under "#{category}"
---
    <div class ="pageBlurb">
        <h1>Postings filed under "#{category}"</h1>
    </div>
    HTML

    html << '<ul class="posts">'
    posts.each do |post|
      post_data = post.to_liquid
      html << <<-HTML
        <li>#{post_data['date'].strftime(fmt="%B %d, %Y")} - <a href="#{post.url}">#{post_data['title']}</a></li>
      HTML
    end
    html << '</ul>'
    
    File.open("categories/#{category}.html", 'w+') do |file|
      file.puts html
    end
  end
  puts 'Done.'
end
