# Updates

## Video Updates

You can check out my YouTube channel here: [https://www.youtube.com/channel/UCvKRFNawVcuz4b9ihUTApCg](https://www.youtube.com/channel/UCvKRFNawVcuz4b9ihUTApCg)

I regularly post updates on YouTube with tutorials and demonstrations.

## Blog Posts

I'll be tracking my progress on Raven here!

{% for post in site.posts %}

### [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
- **{{ post.date | date_to_long_string }}**
- *{{ post.description }}*
<br>

{% endfor %}

