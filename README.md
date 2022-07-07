# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...
# RECORD_carobook<br>
  adding function → like-it, follow(ed), serach(daily & all-pages), <br>
  changes → turbolinks(partial-off), 
 
 
 -- graph --
 
* add chart.js & change turbolinks which can be disable(partial)<br>
  $yarn add chart.js
* + write on application.js(v3 is import specific module)<br>
  import jQuery from "jquery" ← rewrite<br>
  import Chart from 'chart.js/auto';<br>
  global.$=jQuery;<br>
  global.Chart=Chart;<br>
  window.$=jQuery ← cannot search this on any sites(just simmilar ok)
* write turbolinks-can-be-disable on graph view<br>
　ex) → users/show, one of graph<br>
  $(document).on('turbolinks:load', function() {<br>
    var ctx = document.getElementById("myLineChart").getContext('2d');<br>
    var myLineChart = new Chart(ctx, {<br>
        type: 'line',<br>
        data: {<br>
            labels: ['6日前', '5日前', '4日前', '3日前', '2日前', '1日前', '今日'],<br>
            datasets: [{<br>
                label: "投稿数",<br>
                data: [<br>
                 <%= @books.created_6days.count %>,<br>
                 <%= @books.created_5days.count %>,<br>
                 <%= @books.created_4days.count %>,<br>
                 <%= @books.created_3days.count %>,<br>
                 <%= @books.created_2days.count %>,<br>
                 <%= @yesterday_book.count %>,<br>
                 <%= @today_book.count%><br>
                 ],<br>
                backgroundColor: 'rgba(255, 80, 120, 1.0)',<br>
                borderColor: 'rgba(255, 80, 120, 1.0)',<br>
                fill: false<br>
            }]<br>
        },<br>
         options: {<br>
          title: {<br>
            display: true,<br>
            text: '7日間の投稿数の比較'<br>
          },<br>
          scales: {<br>
            yAxes: [{<br>
              ticks: {<br>
                suggestedMax: 10,<br>
                suggestedMin: 0,<br>
                stepSize: 1,<br>
                callback: function(value, index, values){<br>
                  return  value<br>
                }<br>
              }<br>
            }]<br>
          },<br>
        }<br>
    });<br>
  </script>
  
  * add on show-action usersCon<br>
    @today_book =  @books.created_today<br>
    @yesterday_book = @books.created_yesterday<br>
    @this_week_book = @books.created_this_week<br>
    @last_week_book = @books.created_last_week
    
  <br>
  -- table --
  
  * add new-action on usersCon<br>
    content like this ↓<br>
    def ○○○○<br>
    user = User.find(params[:user_id])<br>
    @books = user.books.where(created_at: params[:created_at].to_date.all_day)<br>
    render :○○○○<br>
    end
    
  * write on routing/view<br>
    (routes.rb → while resources users do~end)<br>
    get "○○○○" => "users#○○○○"
    
    (view)※remove<><br>
    div class="my-3"<br>
    %= form_with url: user_daily_search_path(user), method: :get, class: "row justify-content-center", local: false do |f| %<br>
    div class="col-auto"<br>
      %= f.date_field :created_at, class: "form-control" %<br>
    /div<br>
    div class="col-auto"<br>
      %= f.submit "検索" , class: "btn btn-primary"%<br>
    /div<br>
     % end %<br>
    /div<br>
    
    
