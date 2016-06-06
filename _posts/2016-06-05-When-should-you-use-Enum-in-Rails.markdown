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

According to here, enum is:
Declare an enum attribute where the values map to integers in the database, but can be queried by name.

For example, there is a database table call "Payments" and in the table there is a "status" column. The status column type is integer.

Then in the model we can declare an enum "status" which match with your table column name and give an array of values.
 {% highlight ruby %}
 class Payment < ActiveRecord::Base
  enum status: [:success, :failed, :pending]
 end
 {% endhighlight%}
The values will map to the integers in the column of the table, where "success" => 0, "failed" => 1, "pending" => 2.

## Why we want to use enum in our modal?


To Save the space of our database.
  As we can see, the data we store in the "status" column are just integer instead of string. This will help us save a lot of space in our db when the records are getting more and more.

2. We have created more helper methods for our modal!
  When we declare an enum with those values we actually have created the scope for each of its attributes as well.
  For example,

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

We can have declare many enums in a modal. For example, we can create a new collumn called "type" to store the payment type like cash or credit.
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


