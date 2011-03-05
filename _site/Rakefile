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
title: Posts tagged #{tag}
excerpt: Posts tagged #{tag}
---
{% for post in site.posts %}
    {% for tag in post.categories %}
	{% if tag == '#{tag}' %}
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
{% endfor %}
    HTML

    FileUtils.mkdir_p("./search/label/#{tag}")
    File.open("./search/label/#{tag}/index.html", 'w+') do |file|
      file.puts html
    end
  end
  puts 'Done.'
end

desc 'Generate tags cloud'
task :cloud do
  puts "Generating tags cloud..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters

  from = 1
  unto = 6
      
  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  tag_counts = site.categories.map {|tag, posts| [tag, posts.length] }.sort_by {|tag, count| tag.downcase }
  min = tag_counts.min { |a,b| a.last <=> b.last }.last
  max = tag_counts.max { |a,b| a.last <=> b.last }.last
  tag_counts_sizes = tag_counts.map { |tag, count|
      weight = (Math.log(count)-Math.log(min))/(Math.log(max)-Math.log(min))
      if weight.nan?
	weight = 0
      end
      size = from + ((unto-from)*weight).round
      [tag, count, size]
  }

    tag_cloud = ['<ol id="tag-cloud">',
        tag_counts_sizes.map {|t,c,s|
          title = c == 1 ? "1 post" : "#{c} posts"
          %{<li title="#{title}"><a href="/search/label/#{t}">#{t}</a></li>}
        }.join(' '),
      '</ol>'
    ].join
    html = ''
    html << <<-HTML
    #{tag_cloud}
    HTML

    FileUtils.mkdir_p("./_includes/")
    File.open("./_includes/tag_cloud.textile", 'w+') do |file|
      file.puts html
    end
  puts 'Done.'
end