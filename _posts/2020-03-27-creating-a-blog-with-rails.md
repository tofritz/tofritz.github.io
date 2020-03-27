---
layout: post
title:  "a blog using rails & heroku deployment"
author: tyler
date:   2020-03-027 18:00:00 -0600
categories: odin ruby rails
---

This post will cover the deliverables I've produced while working through the section of the `Web Dev 101` course which introduces Web Development Frameworks. The lessons introduce what frameworks are, how they facilitate development, some of the most popular frameworks, and the principles behind how some of them are structured.

The example project for this section focused on utilizing Ruby on Rails to teach some of the basic concepts behind frameworks. One of the driving reasons to use a framework and why they help facilitate web development is that they enforce modularity. This modularity can: 
- enhance organization and hierarchy
- aid debugging through module testing
- aid debugging by providing common structures
- prevent repeated code and therefore lead to a more efficient codebase

I understood what frameworks were before I completed this project, but diving into making the Rails app itself helped clarify exactly what Model-View-Controller separation actually looks like in the code itself. Additionally, REST APIs and RESTful-type software was something I knew conceptually. However, this exercise was very useful in getting a hands-on learning experience with RESTful architecture and how that design integrates an application's code interoperates through the browser via HTTP. It is really useful to a relatively few number of protocol verbs like `POST` or `GET` can lead to innumerable interactions with an application just by routing those requests through certain defined URL pathways that lead to different sections of application code. For example, a snippet of `rake routes` for my project app:

```
Prefix          Verb    URI Pattern                   Controller#Action
.
.
.
articles        GET    /articles(.:format)            articles#index
                POST   /articles(.:format)            articles#create
new_article     GET    /articles/new(.:format)        articles#new
edit_article    GET    /articles/:id/edit(.:format)   articles#edit
article         GET    /articles/:id(.:format)        articles#show
                PATCH  /articles/:id(.:format)        articles#update
                PUT    /articles/:id(.:format)        articles#update
                DELETE /articles/:id(.:format)        articles#destroy
tags            GET    /tags(.:format)                tags#index
                POST   /tags(.:format)                tags#create
.
.
.
```
So this routes configuration show that my app will take any of those HTTP verbs pointed at those URLs and they will route it to an appropriate controller with packaged parameters. In this case, the articles controller:
```ruby
class ArticlesController < ApplicationController
    include ArticlesHelper
    before_action :require_login, except: [:show, :index]

    def index
        @articles = Article.all
    end

    def show
        @article = Article.find(params[:id])

        @comment = Comment.new
        @comment.article_id = @article.id
    end

    def new
        @article = Article.new
    end

    def edit
        @article = Article.find(params[:id])
    end

    def create
        @article = Article.new(article_params)
        @article.save

        flash.notice = "Article '#{@article.title}' Created!"

        redirect_to article_path(@article)
    end

    def update
        @article = Article.find(params[:id])
        @article.update(article_params)

        flash.notice = "Article '#{@article.title}' Updated!"

        redirect_to article_path(@article)
    end
    
    def destroy
        @article = Article.destroy(params[:id])

        flash.notice = "Article '#{@article.title}' Deleted!'"

        redirect_to articles_path
    end
end
```

Anyway, I'll stop myself from rehashing the entire lesson. I used Rails in this project to create typical blogging application. It has things like articles, comments, tags, login authentication, post attachments, etc. I actually did not get into the weeds with doing some of the extra 'bonus' suggestions for building out the app because:
- I prefer this `jekyll` system for the type of infrequent posting I'm doing. It also integrates well already with my github and I don't have to worry about deployment.
- I prefer Python for my day to day work and therefore if I am doing deeper dives/troubleshooting then I will try to do that learning in the Django framework

The repository for my Rails app can be found in my [`rails-blogger`](https://github.com/tofritz/rails-blogger) repository. Additionally, a live version can be found as a [Heroku deployment](https://protected-sierra-34418.herokuapp.com/). This helped me learned the basics of the Heroku CLI tool and basically how painless it is to just `git push heroku master`.