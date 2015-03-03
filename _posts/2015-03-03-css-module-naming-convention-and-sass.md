---
layout: post
title: CSS Module Naming Convention and Sass
category: SASS
---

This isn't news. But I haven't heard of it until some days ago:

Since *Sass 3.3.0* "the parent selector, &, can be used with an identifier suffix"!  
What this means is, that it's possible to write something like this

{% highlight scss %}
.vehicle {
    opacity: 0.9; // somehow the client wants this

    &--car {
        wheels: 4;

        &--fast {
            filter: blur(9px);
        }
    }

    &--truck {
        &--extra-long {
            &--trucker-seat {
                &--leather {
                    cosy: 'very';
                }
            }
        }
    }
}
{% endhighlight %}

which will become this

{% highlight css %}
.vehicle {
    opacity: 0.9;
}

.vehicle--car {
    wheels: 4;
}

.vehicle--car.vehicle--fast {
    filter: blur(9px);
}

.vehicle.vehicle--truck.vehicle--truck--extra-long.vehicle--truck--extra-long--trucker-seat.vehicle--truck--extra-long--trucker-seat--leather {
    cosy: 'very';
}
{% endhighlight %}


So the only one to write longer and longer class names is the poor template/html guy.

In the [changelog](http://sass-lang.com/documentation/file.SASS_CHANGELOG.html) they mentioned this under "Smaller improvements". I think this is a huge improvement!

Yeah!