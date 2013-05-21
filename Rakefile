# -*- coding: utf-8 -*-

task :default => :build

desc "Build your site"
task :build do
  `jekyll build`
end
ENV['title'] = "dummy-title" if ENV['title'].nil?
NOW = Time.new.strftime "%Y-%m-%d %H:%M:%S"
TODAY = Time.new.strftime "%Y-%m-%d"
POST_TEMP = <<-HEAD
---
layout: post
title:  "xxxxxxx"
date:   #{NOW}
categories: tag
---

HEAD


desc "Create yyyy-mm-dd-title entry markdown"
task :post => "./_posts/#{TODAY}-#{ENV['title']}.markdown"

file "./_posts/#{TODAY}-#{ENV['title']}.markdown" do |f|
  open(f.name, "w"){|x| x << POST_TEMP}
end
