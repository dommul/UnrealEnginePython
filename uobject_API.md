The uobject API
===============

This is the list of the methods exposed by the uobject api.

Remember we do not need to expose all of the methods available in ue4, but only the most common one that could impact performances
if using the c++ reflection too much. For all the other cases using uobject.call('Function args') is the blessed approach.

---
```py
x, y, z = uobject.get_actor_location()
```

get the current actor location (automatically retrieve the actor from the component if needed)

---
```py
uobject.set_actor_location(x, y, z)
```

set the current actor location (automatically retrieve the actor from the component if needed)

---
```py
pitch, yaw, roll = uobject.get_actor_rotation()
```

get the current actor rotation (automatically retrieve the actor from the component if needed)


---
```py
uobject.set_actor_rotation(pitch, yaw, roll)
```
set the current actor rotation (automatically retrieve the actor from the component if needed)

---
```py
x, y, z = uobject.get_actor_forward()
```

get the current actor forward vector (automatically retrieve the actor from the component if needed)

---
```py
x, y, z = uobject.get_actor_right()
```

get the current actor right vector (automatically retrieve the actor from the component if needed)

---
```py
x, y, z = uobject.get_actor_up()
```

get the current actor up vector (automatically retrieve the actor from the component if needed)

---
```py
x, y, z = uobject.get_actor_velocity()
```
get the current actor velocity vector (automatically retrieve the actor from the component if needed)


---
```py
value = uobject.get_property('name')
```

get the property value of a uobject

NOTE: currently structs are not supported


---
```py
uobject.set_property('name', value)
```

set the property of a uobject

NOTE: currently structs are not supported

---
```py
properties_list = uobject.properties()
```

get the list of properties available for the uobject (as a list of string names)

---
```py
uobject.call('function arg0 arg1 argN....')
```

this is probably the most important functionality of the Unreal Engine reflection system.

It allows you to call a method/function exposed by a uobject passing it arguments.

For example you have an uobject mapped to a UTextRenderComponent and you want to change the text:

```py
text_render_component.call('SetText Hello')
```

will call the UTextRenderComponent::SetText(FString value) method passing 'Hello' as the string argument

This methods allows you to interface with basically every engine features, but is is obviously slower than directly calling C++ methods.

Use it for calling blueprint functions, or while waiting for the addition of new specific-methods in the uobject api ;)

---
```py
ret = uobject.call_function('function', arg0, arg1, argN....')
```

the advanced companion of 'call'. This is a more advanced way for calling UFUNCTION's and for getting their return value.

Note: structs are not supported

---
```py
actor = uobject.get_owner()
```

get the owner of an object (generally called by a component to get its actor)

---
```py
world = uobject.get_world()
```

get the world containing an object. (currently useful only for testing and comparison)

---
```py
name = uobject.get_name()
```
get the name of the uobject

---
```py
full_name = uobject.get_full_name()
```

get the full name of the uobject

---
```py
# really slow !
found_uobject = uobject.find_object('name')
```

find an object (by name) in the same world of the caller (SLOW, do not use it unless you know what you are doing)

---
```py
# really really slow !
found_uobjects = uobject.all_objects()
```

get the list of all uobjects in the same world of the caller (REALLY SLOW,  do not use it unless you know what you are doing)

---
```py
found_actors = uobject.all_actors()
```

get the list of all actors available in the same world of the caller. A bit slow.

---
```py
uclass = uobject.get_class()
```

this is the same as calling ->GetClass() in c++. You can get the UClass of a uobject.

---
```py
uclass = uobject.actor_spawn(uclass[, x, y, z, pitch, yaw, roll])
```

spawn an actor. uclass is the reference you can get via get_class() or unreal_engine.find_class()

---
```py
uobject.actor_destroy()
```

destroy an actor

---
```py
yesno = uobject.actor_has_component_of_type(uclass)
```

return True if the actor has a component of the specified type

---
```py
yesno = uobject.get_actor_component_by_type(uclass)
# alias
yesno = uobject.get_component_by_type(uclass)
```

return the first component (of an actor) of the specified type

It has two shortcuts, it can retrieve the actor from a component, and can get a string (instead of the output of ue.find_class('name')) with the class of the component

---
```py
components = uobject.actor_components()
```

get the list of all components mapped to the actor

---
```py
uobject.enable_input()
```

enable the input system on an object

---
```py
uobject.bind_input_axis('axis')
```

---
```py
uobject.quit_game()
```

well, quit the game :)

---
```py
yesno = uobject.is_input_key_down('key')
```

---
```py
value = uobject.get_input_axis('axis')
```

---
```py
yesno = uobject.actor_has_tag('tagname')
```

check if an actor is tagged with the specific tag

---
```py
x, y, z, ex, ey, ez = uobject.get_actor_bounds()
```

get the bounds of an object (ex, ey and ez are the extents)

---
```py
hit_object, x, y, z, nx, ny, nz = uobject.line_trace_single_by_channel(x0, y1, z0, x1, y1, z1, channel)
```

---
```py
[(hit_object, x, y, z, nx, ny, nz), ...] = uobject.line_trace_multi_by_channel(x0, y1, z0, x1, y1, z1, channel)
```

---
```py
uobject.show_mouse_cursor()
```

---
```py
uobject.enable_click_events()
```

---
```py
uobject.enable_mouse_over_events()
```

---
```py
uobject.destructible_apply_damage(damage, impulse, dx, dy, dz, ix, iy, iz)
```

See Fracturing section on the README

---
```py
uobject.set_view_target(target)
```

change the view target. The 'target' argument should be an actor with a camera component.

---
```py
uobject.set_simulate_physics()
```

---
```py
new_component = uobject.add_actor_component(uclass, 'name')
```

add a new component of the specified uclass (type) and set its name

---
```py
new_component = uobject.add_actor_root_component(uclass, 'name')
```

add a new component as the root one of the specified uclass (type) and set its name

---
```py
uobject.simple_move_to_location(x, y, z)
```

move to a location using navmesh (see Navigation on main README)

---
```py
clicked_actor, x, y, z, nx, ny, nz = self.uobject.get_hit_result_under_cursor(channel)
```

get the world point under the mouse cursor (see Navigation on main README, for an example usage)


---
```py
length = uobject.get_spline_length()
```

get the length of a spline component (see Spline section in the main README)

---
```py
x, y, z = uobject.get_world_location_at_distance_along_spline(distance)
```

get the a point of a spline component based on distance (see Spline section in the main README)
