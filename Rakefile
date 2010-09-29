desc 'Generate tags page'
task :tags do
  puts "Generating tags..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters

  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  site.categories.each do |tag, posts|
    html = ''
    html << <<-HTML
---
layout: default
title: Posts tagged "#{tag}"
excerpt: Posts tagged "#{tag}"
---

{% assign tag = #{tag} %}
{% for post in posts %}
    {% if post.categories.include?(tag) %}
  <div class="post">
    <h3><a href="{{ post.url }}/">{{ post.title }}</a></h3>
    <p class="date">{{ post.date | date_to_string }}
      {% if post.author %}
        by <a href="http://twitter.com/{{ post.author.twitter }}" title="{{ post.author.full}}">@{{ post.author.twitter }}</a>
      {% endif %}
    </p>
    <p>{{ post.excerpt }}</p>
  </div>
    {% endif %}
{% endfor %}
    HTML

    FileUtils.mkdir_p("./search/label/#{tag}")
    File.open("./search/label/#{tag}/index.html", 'w+') do |file|
      file.puts html
    end
  end
  puts 'Done.'
end