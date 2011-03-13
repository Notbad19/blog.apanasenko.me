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
<style type="text/css">
#content ul, 
#content li {
	display: inline;
	margin: 0; padding: 0;
}

#content li {
	width:266px;
	height: 530px;
	float:left;
	position:relative;
}

#content span>a {
	font-size:16px;
	font-weight:bold;
	line-height:12px;
	color: #3d393a;
}

#content .tag a {
	font-size: 12px;
	color:#ac1111;
}

#content a {
	text-decoration:none;	
}

#content a:hover {
	text-decoration:underline;
}

.cs_wrapper p {
	width: 250px;
	font-size: 14px;
}
</style>

{% for post in site.posts %}
    {% for tag in post.categories %}
	{% if tag == '#{tag}' %}
  <div class="cs_wrapper">
  	<div class="cs_slider">
  		{% for post in site.posts %}
  		{% if 0 == forloop.index0  or 6 == forloop.index0 or 12 == forloop.index0 or 18 == forloop.index0 or 24 == forloop.index0 %}
  		<div class="cs_article">
  			<ul class="entry-content">
  		{% endif %}
  				<li>
  			  		<div>
  			    		<span><a href="{{ post.url }}/">{{ post.title }}</a></span>
  						<br/><br/>
  						{% if post.image %}
  						<div class="imgLarge wd205"> 
  							<a href="{{ post.url }}/"><img width="175" height="175" src="{{ post.image }}" alt="" /></a>
  						</div>
  						{% else %}
  						<div class="imgLarge wd205"> 
  							<a href="{{ post.url }}/"><img width="175" height="175"/></a>
  						</div>
  						{% endif %}
  			    		<p>{{ post.excerpt }}</p>
  						<br/><br/>
  			  		</div>
  				</li>
  		{% if 5 == forloop.index0  or 11 == forloop.index0 or 17 == forloop.index0 or 23 == forloop.index0 %}
  			</ul>						
  		</div>
  		{% endif %}
  		{% endfor %}
  	</div>
  </div>
  <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.3.2/jquery.min.js"></script>
  <script type="text/javascript" src="/js/jquery.easing.1.3.js"></script>
  <script type="text/javascript" src="/js/jquery.ennui.contentslider.js"></script>
  <script type="text/javascript">
  	$(function() {
  		$('#content').ContentSlider({
  			width : '850px',
  			height : '1000px',
  			speed : 400,
  			easing : 'easeOutQuad',
  			// textResize : true
  		});
  	});
  </script>	
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