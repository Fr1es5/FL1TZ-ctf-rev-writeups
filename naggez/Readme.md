  # naggez Challenge
## Description: missleading
## Recommended tool: 
- GDRE_tools.
## Important note:
- In this challenge both the linux and windows executable formats weer provided. This writeup will use **the windows executable**.
## Steps to solve:
- When opening the game, we notice that the player is a crying emoji (bruh) moving on a platform and scattered flags and letters are all over the interface of the game. (check screenshot_1)
- When interacting with the game you can access the far right flag and when you do its color changes from the green to red and the scattered letters and numbers change positions, so I assumed that when we reach all flags in a
- certain order we might get the letters to certain positions so that they form the flag.
- But when moving to the far left we notice there are invisible obstacles with no way to pass or jump over. So I resorted to checking the code behind the movement of the letters using **GDRE_tool.**
- - This tool allows you to access all related assets behind the game. Under the RE tools you choose recover project and select the naggeez.exe file then select full recovery. (check screenshot_2)
  - After exploring the files I took interest in **mc.gd** and **arena.tscn** so let’s open then.  You can open them witht he corresponding editors. I opened them using notepad xD.
    - Starting with **arena.tscn**, I found all the interesting parts needed to get the flag: the code behind the movement of the letters, variables assigned to each flag and the variables assigned to each letter.
      Here are they in order:
      ```[},I,t,_,1,L,t,!,S,O,4,b,t,t,4,n,{,0,d,F,o,h,Z,_,d,T,_,g,XXX]```
      And here is the code in question:
      ```
      func _process(delta):
          var flag_states = []
          for flag in flag_objects:
          flag_states.append(int(flag.target_state))
          var concatenated_flags = \"\".join(flag_states)
          var character_nodes = %FlagText.get_children()
          var transformation_values = [0x10e, -0x32, 0xaa, -0x46, -0xfa, -0x10e, 0x46, 0xfa, -0x1e, -0x6e, 0xd2, 0xbe, 0x6e, -0x5a, 0x96, 0x1e, -0xbe, -0x96, 0xe6, -0x122, 0x32, 0x82, -0xd2, -0xa, -0x82, -0xe6, 0x5a, -0xaa, 0xa]
          #1111011111 
          if (concatenated_flags != \"1111011111\"):
              var hash_value = concatenated_flags.sha1_text().to_upper()
              hash_value += concatenated_flags.md5_text().to_upper()
              character_nodes[0].target_y = 0
              character_nodes[1].target_x = 1
              for i in character_nodes.size():
                  character_nodes[i].target_x = convert_hex_to_int(hash_value.unicode_at(i * 2)) - 8
                  character_nodes[i].target_y = convert_hex_to_int(hash_value.unicode_at((i * 2) + 1)) - 8
        else:
            var hash_value = concatenated_flags.sha1_text().to_upper()
            hash_value += concatenated_flags.md5_text().to_upper()
            for i in character_nodes.size():
                character_nodes[i].target_x = transformation_values[i]
                character_nodes[i].target_y = convert_hex_to_int(hash_value.unicode_at((i * 2) + 1)) - 8
                var unused_variable
                for loop_x in range(-290, 290, 20):
                    var loop_index = 0
                    for character_node in character_nodes:
                        if character_node.target_x == loop_x:
                            if character_node.text != \"XXX\":
                                print(\"_\\nb\" if character_node.text == \"b\" else character_node.text)
      ```
      Pretty long but we will only focus on a certain part. Let's try to understand it:
      -
      ```
      var flag_states = []
          for flag in flag_objects:
          flag_states.append(int(flag.target_state))
          var concatenated_flags = \"\".join(flag_states)
          var character_nodes = %FlagText.get_children()
          var transformation_values = [0x10e, -0x32, 0xaa, -0x46, -0xfa, -0x10e, 0x46, 0xfa, -0x1e, -0x6e, 0xd2, 0xbe, 0x6e, -0x5a, 0x96, 0x1e, -0xbe, -0x96, 0xe6, -0x122, 0x32, 0x82, -0xd2, -0xa, -0x82, -0xe6, 0x5a, -0xaa, 0xa]
          #1111011111
      ```
        - This part basically tells us that there is a code that assigns each state of the flags (red or green) 0 or 1 and It append each value into a string. 
          character_nodes is an array  that takes the letters of the flag in the order I provided previously 
          Transformation_values is an array that contains values required to get the flag.
          Since the correct concatenated_flags value is 1111011111 we will ignore the if (concatenated_flags != \"1111011111\"): part and jump to else part.
     -
      ```
      else:
            var hash_value = concatenated_flags.sha1_text().to_upper()
            hash_value += concatenated_flags.md5_text().to_upper()
            for i in character_nodes.size():
                character_nodes[i].target_x = transformation_values[i]
                character_nodes[i].target_y = convert_hex_to_int(hash_value.unicode_at((i * 2) + 1)) - 8
                var unused_variable
                for loop_x in range(-290, 290, 20):
                    var loop_index = 0
                    for character_node in character_nodes:
                        if character_node.target_x == loop_x:
                            if character_node.text != \"XXX\":
                                print(\"_\\nb\" if character_node.text == \"b\" else character_node.text)
      ```
      - If we analyze the code overally, it basically assigns each target_x and target_y of each flag character a certain value. 
        We can find the target_x and target_y in the mc.gd file and each of them are initiated at 0.
        We will only take interest in target_x values because the most important part of the code (the next code) only manipulates target_x of each flag character gets the values of transformation_values in order so let’s convert them:
        ```
        [270, -50, 170, -70, -250, -270, 70, 250, -30, -110, 210, 190, 110, -90, 150, 30, -190, -150, 230, -290, 50, 130, -210, -10, -130, -230, 90, -170, 10]
        ```
      - Jumping to what's actually important:
        ```
        for loop_x in range(-290, 290, 20):
                    var loop_index = 0
                    for character_node in character_nodes:
                        if character_node.target_x == loop_x:
                            if character_node.text != \"XXX\":
                                print(\"_\\nb\" if character_node.text == \"b\" else character_node.text)
        ```
        - This loop checks if each target_x  of the corresponding character_node matches the loop_x values and prints the characters of the flag accordingly. if the character is XXX it will ignore it. Since the loop starts at -290 and ends at 270 and has a step of 20 the values will match transformation values.
        For example:  if we take the character F at the position 19
        ```
        [},I,t,_,1,L,t,!,S,O,4,b,t,t,4,n,{,0,d,F,o,h,Z,_,d,T,_,g,XXX]
        ```
        Its target_x will take the value of transformation_values at position 19 meaning -290.
        ```
        [270, -50, 170, -70, -250, -270, 70, 250, -30, -110, 210, 190, 110, -90, 150, 30, -190, -150, 230, -290, 50, 130, -210, -10, -130, -230, 90, -170, 10] 
        ```
        - and so according to the loop since its starts at -290 it will print F first, which is the start of the ctf flag! Following this logic to each target_x of each character in character_nodes we will get the flag.
          Here is a python script just for that:
          ```
          character_nodes = ['}', 'I', 't', '_', '1', 'L', 't', '!', 'S', 'O', '4', 'b', 't', 't', '4', 'n', '{', '0', 'd', 'F', 'o', 'h', 'Z', '_', 'd', 'T', '_', 'g', 'XXX']
          target_x = [270, -50, 170, -70, -250, -270, 70, 250, -30, -110, 210, 190, 110, -90, 150, 30, -190, -150, 230, -290, 50, 130, -210, -10, -130, -230, 90, -170, 10]
          result = ""
          for loop_x in range(-290, 290, 20):
              for index, character in enumerate(character_nodes):
                  if target_x[index] == loop_x:
                      if character != "XXX":
                          result += "_b" if character == "b" else character
          print( result)
          ```
          



        
        
      
        

      



