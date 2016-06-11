---
layout: post
permalink: /:title
title:  "When should you use Enum in Rails"
date:   2016-06-05
categories: jekyll update
excerpt_separator: <!--more-->
---

## What is enum in Rails?
<!--more-->

According to [here](http://edgeapi.rubyonrails.org/classes/ActiveRecord/Enum.html), enum is:

> Declare an enum attribute where the values map to integers in the database, but can be queried by name.

For example, there is a database table call <b>"Payments"</b> and in the table there is a <b>"status"</b> column. The column type is <b>Integer</b>.

Then in the model we can declare an enum "status" that match with our column name and give some values.
 {% highlight ruby %}
 class Payment < ActiveRecord::Base
  enum status: [:success, :failed, :pending]
 end
 {% endhighlight%}
The values will map to the integers in the column of the table, where "success" => 0, "failed" => 1, "pending" => 2.

## Why we want to use enum in our modal?


#### To Save the space of our database.

  As we can see, the data we store in the "status" column are just integer instead of string. This will help us save a lot of space in our db when the records are getting more and more.

#### Enum helps to create more helper methods for our modal!

When we declare an enum with those values we actually have created the scope for each of its values as well. For example,

{% highlight ruby %}
payment = Payment.new
payment.success #will return all payment where status equal to success
payment.failed  #will return all payment where status equal to failed
{% endhighlight %}

Besides that, we can check the status of the record.
For example,

{% highlight ruby %}
payment = Payment.new(status: :success)
payment.success? #return true
payment.failed?  #return false
{% endhighlight %}

And, we can update the status of the record as well.
{% highlight ruby %}
payment = Payment.new
payment.success! 
payment.success? #return true
{% endhighlight %}

We can declare as many enums in a modal as we want. Let create a new collumn called "type" to store the payment type like cash or credit.
{% highlight ruby %}
 class Payment < ActiveRecord::Base
  enum status: [:success, :failed, :pending]
  enum type: [:cash, :credit], _prefix: :paid_by
 end
{% endhighlight %}

we can make it more narrative by adding prefix/suffix to the enum.
{% highlight ruby %}
  payment = Payment.new(type: :cash)
  payment.paid_by_cash? #return true
{% endhighlight %}

One more thing, we can show all the available enum values by calling the pluralized name of the column. 
{% highlight ruby %}
  Payment.statuses #return {"success"=>0, "failed"=>1, "pending"=>2}
{% endhighlight %}


