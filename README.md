hrml
====

So I made a language.

It's closely based on [HAML](http://haml-lang.com) and I couldn't think of a name so I called it HRML, as in "ermahgerd, hrml". Pronounced "hurr-məl".

I don't like Ruby. I'll just say it. I don't totally know why, but something about its code aesthetic and black-box philosophy makes me deeply uncomfortable. I like **Python**. A lot. So much. So when I stumbled across HAML, I saw it instantly to be very... well, Ruby. Rubiful, perhaps. And I wanted it to be more Pythonic. I wanted to use it with Django.

So, HAML says it makes "code haiku"; but it's still full of sigils and unnecessary bloat. That's not very Pythonic. Let's take their example:

    #profile
      .left.column
        #date= print_date
        #address= current_user.address
      .right.column
        #email= current_user.email
        #bio= current_user.bio

This is also (almost) valid HRML. There's no facility for the `#email=` variable evaluation, because this is (sort of) platform agnostic. I suppose you could add it in, if you wanted; it wouldn't be very difficult.

But let's take a look at something that's more common; something not made entirely of div tags. How about a login screen?

    %p
      %span.red Service X
      is for staff only.
    %table.center
      %tr
        %th{:scope => "row"} Staff ID
        %td
          %input#staffid{:type => "text"}/
      %tr
        %th{:scope => "row"} Password
        %td
          %input#password{:type => "password"}/
      %tr
        %td
        %td
          %input{:type => "submit", :value => "Login"}/
      
Now that's some top-quality BS right there. What's going on with the attributes? The percentage signs? It's awful. Let's see the same thing in HRML.

    p
      span.red Service X
      : is for staff only.
    table.center
      tr
        th scope=row Staff ID
        td
          input #staffid type=text /
      tr
        th scope=row Password
        td
          input #password type=password /
      tr
        td
        td
          input type=submit value=Login /

Ah. Like a fresh breeze. First of all we're using the far more familiar name=value attribute style from HTML. For one-word values, the quotes are optional, getting rid of a lot of visual noise. The first word of a line is assumed to be an HTML tag, and the rest of it the contents. If the line starts with `-` it's interpreted as a code block and if it starts with `=` it's interpreted as variable evaluation. Since I wrote this with Django in mind, `- block content` becomes `{% block content %}{% endblock %}`. `- extends 'layout.hrml' /` becomes `{% extends 'layout.hrml' %}`. `= row.value` becomes `{{ row.value }}`.

Like HAML, we have the magic attributes for `id` and `class`. In the above example `span.red Service X` becomes `<span class="red">Service X</span>`. `input #staffid type=text /` becomes `<input id="staffid" type="text" />`. The trailing slash, like HAML, indicates an empty, or self-closing, tag. This applies for Django code block tags as well (as you can see in the previous paragraph.) Also like HAML, if you simply start a tag with a class or ID, it's assumed to be a div. Unlike HAML, however, you can fill out your classes and IDs with as much whitespace as you like.

Finally, a line begining with `:` becomes plain text, as does any further indented text below it. This means it is passed straight through without parsing - so you can put in HTML, or Django template tags, or whatever. You can see this in the line `: is for staff only.` above.

But it's not *just* plain text: it's **Markdown**! Stick a `**bold**` or `*italic*` in there and it becomes `<strong>bold</strong>` and `<em>italic</em>`. Other Markdown stuff probably *should* be implemented but isn't yet.

There's no syntactic magic, either. `!!!` doesn't becomes a DOCTYPE. Write it yourself. It's just `: <!DOCTYPE html>` for HTML 5.

There's also no comments, which is something I should probably rememdy in the future. I was going to have it so that lines starting with `#` were comments, but that clashes with the implicit-div rule. If you have an idea on how to do comments, please let me know.

**So that's it! Enjoy HRML.**



PS Yes I know there is already a language called HRML. Sue me.