---
layout: post
title: Godot - Simple Trail Effect
modified:
categories: godot
description:
tags: [godot, gamedev]
image:
  feature:
  credit:
  creditlink:
comments:
share:
date: 2018-12-16T07:30:57-05:00
---

## How it started
Some months ago I started playing around with <a href="https://godotengine.org/" target="_blank">Godot engine</a>, I've started with a Pong clone
just to learn the basics, then - when talking about it to a friend - came the idea of this game where the player reflects 
projectiles with some sort of magic shield. And here I am, making some good progress (considering the short amount of free time 
I have) thanks to how high level development Godot provides, its <a href="https://godotengine.org/community" target="_blank">awesome community</a>, 
<a href="https://docs.godotengine.org/en/3.0/getting_started/scripting/gdscript/gdscript_basics.html" target="_blank"> great documentation</a> and 
great learning references, such as <a href="https://www.youtube.com/channel/UCxboW7x0jZqFdvMdCFKTMsQ" target="_blank">GD Quest</a>!

## The dash skill and its trail effect
{% include youtubePlayer.html id="a4PSn7kxl_o" %}
### The problem:  
I have this modular character composed by head and body sprites, it doesn't have multiple animation frames,
its animations are just position and scale <a href="https://docs.godotengine.org/en/3.0/classes/class_tween.html?highlight=tween" target="_blank">tweens</a>.  

### My solution:  
I've created a script and attached it to a child node of my character. This dash script has a `owner_path` that should point to the character. It
assumes the character has some methods:

{% highlight plaintext %}
# Character.gd
func get_speed():
  return _speed

func set_speed(value):
  _speed = value

func set_input_enabled(enabled):
  _input_enabled = enabled

func get_current_sprites():
  var head = get_node("YSort/Node2D/Head")
  var body = get_node("YSort/Node2D/Body")
  return [head, body] 
{% endhighlight %}

Then it uses those methods to achieve the dash with trail effect: 

{% highlight plaintext %}
# Dash.gd
extends Node

# The owner of this Dash node needs to have the following methods:
# func set_input_enabled(enabled:bool)
# func get_speed()
# func set_speed(speed_scalar)
# func get_current_sprites() -> List[Sprite]

export (NodePath) var owner_path
var owner_body
var owner_speed

# list of list: each list contains all necessary sprites to represent a 
# ghost copy of the original character
var ghosts = [] 
var tween

func _ready():
    owner_body = get_node(owner_path)
    var num_sprites = owner_body.get_current_sprites().size()
    for i in range(5): # 5 ghost trails
        var sprites = []
        for j in range(num_sprites): 
            var s = Sprite.new()
            s.set_visible(false)
            add_child(s) 
            sprites.append(s)
        ghosts.append(sprites)

    tween = Tween.new()
    add_child(tween)

func dash():
    owner_body.set_input_enabled(false)
    owner_speed = owner_body.get_speed()
    owner_body.set_speed(owner_speed * 3)

    var timer = Timer.new()
    timer.connect("timeout", self, "_return_input_to_owner") 
    add_child(timer) 
    _start_ghost_tweens()
    timer.start(0.25)
    timer.set_one_shot(true)

func _start_ghost_tweens():
    for i in range(ghosts.size()): # ghost trails
        yield(get_tree().create_timer(0.05), "timeout")

        for j in range(ghosts[i].size()): # ghost parts (head, body, etc)    
            var owner_sprites = owner_body.get_current_sprites() 
            var ghost_part = ghosts[i][j]

            ghost_part.set_scale(owner_body.global_scale)
            ghost_part.set_position(owner_sprites[j].global_position)
            ghost_part.set_texture(owner_sprites[j].get_texture())
            ghost_part.set_rotation(owner_sprites[j].global_rotation)
            ghost_part.flip_h = owner_sprites[j].flip_h
            ghost_part.set_visible(true)

            tween.interpolate_property(
                ghost_part, 
                "modulate", 
                Color(1, 1, 1, 1), 
                Color(1, 1, 1, 0), 
                0.25, 
                Tween.TRANS_LINEAR, 
                Tween.EASE_IN
            )
            if not tween.is_connected("tween_completed", self, "_on_complete_ghost_tween"):
                tween.connect("tween_completed", self, "_on_complete_ghost_tween") 
            tween.start()

func _on_complete_ghost_tween(object, key):
    object.set_visible(false)

func _return_input_to_owner():
    owner_body.set_speed(owner_speed)
    owner_body.set_input_enabled(true)
{% endhighlight %}

## Next
You can easily change this script to `export` the number of ghost trails and tween variables so
you can change them directly on the editor. Also, it might be a good idea to detach (by creating another script)
the trail from the dash logic, after all you might want to add the trail effect to other things that won't have a dash skill.


Thanks for reading! 

<script type='text/javascript' src='https://ko-fi.com/widgets/widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Buy me a coffee', '#151a19', 'M4M4NEUK');kofiwidget2.draw();</script> 