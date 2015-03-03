Rails Before Filters
==============================

The purpose of before filters (now called before actions) is to filter access to
controller actions. This makes them a good solution for things like
authorization, but not a good one for things like populating instance variables.

The Good
--------------------

DO the following

```ruby
class GoodController < ApplicationController
  before_filter :authorize, :only => :show

  def index
  end

  def show
  end

  private

  def authorize
    unless user_signed_in?
      raise NotAuthorizedError
    end
  end
end
```

No state changes, but in certain circumstances the entire action needs to be
avoided.

The Not So Good
----------------------

Avoid the following

```ruby
class NotSoGoodController < ApplicationController
  before_filter :find_post, :only => [:show, :edit, :create]

  def show
  end

  def edit
  end

  def create
  end

  private

  def find_post
    @post = Post.find(params[:id])
  end
end
```

While this reads like you've DRYed up the code, parsing it requires reading the
action, going to the before filters, seeing what instance variables get set
there, and then going back to the action. The flow is broken, and thus it's much
harder to keep in your head. Prefer something like below:

```ruby
class BetterController < ApplicationController
  def show
    @post = find_post
  end

  def edit
    @post = find_post
  end

  def create
    @post = find_post
  end

  private

  def find_post
    Post.find(params[:id])
  end
end
```

This looks like more code, but the knowledge of HOW to find a post has been
extracted, and assigning instance variables is now local to the action and thus
easier to understand.
