# -*- coding: utf-8 -*-
require "rake/clean"

CLEAN << "_site/**/*"

ENV['title'] = "dummy-title" if ENV['title'].nil?
NOW = Time.new.strftime "%Y-%m-%d %H:%M:%S"
TODAY = Time.new.strftime "%Y-%m-%d"

POST_TEMP = <<-HEAD
---
layout:     post
title:      "#{ENV['title']}"
date:       #{NOW}
categories: #{ENV['cat']}
tags:       #{ENV['tag']}
---

HEAD

task :default => :build

desc "Build your site(:default)"
task :build do
  `jekyll build`
end


desc "Create yyyy-mm-dd-title entry markdown"
task :post => "./_posts/#{TODAY}-#{ENV['title'].gsub(/\s/,"-")}.markdown"

file "./_posts/#{TODAY}-#{ENV['title'].gsub(/\s/,"-")}.markdown" do |f|
  open(f.name, "w"){|x| x << POST_TEMP}
end
