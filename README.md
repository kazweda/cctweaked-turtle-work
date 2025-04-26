# cctweaked-turtle-work

### gps
First of all, we need a gps.
[Setting up GPS](https://tweaked.cc/guide/gps_setup.html)

We need the absolute positions to move turtles.
Please setup gps and check positions of workspace.

### setup turtle
Set computer label for each turtle.(t1 .. t10)
I made turtles by copying, then turtle IDs are all same number.
So I need to set labels for each turtle.
<img width="503" alt="スクリーンショット 2025-04-26 21 25 37" src="https://github.com/user-attachments/assets/554606a2-361e-4154-beb4-c10cbd9d9a2e" />

Each turtle needs
- Coals ( for fuel )
- Concretes
- Wireless Modem
- Diamond Pickaxe 
<img width="832" alt="スクリーンショット 2025-04-26 21 19 57" src="https://github.com/user-attachments/assets/16b86aa1-202a-49ea-8bc0-80a5e56e9f43" />

#### Receiver program for turtles
Install [receivepos2.lua](https://pastebin.com/a4rUkCFn) as startup to each turtle.
If you turn on any turtle, this program starts.

#### main loop for turtles
This is a part of startup(receivepos2.lua).

For example, t1(turtle No.1) gets a message of
```
"t1,296,67,-255"
```
move to 296, 67, -155.
Each turtle is always waiting messages like this.

```lua
rednet.open("left")
print("name:"..os.getComputerLabel())
print("fuel:"..turtle.getFuelLevel())
 
while true do
  if turtle.getFuelLevel() < 80 then
    turtle.refuel()
  end
  
  local sid, msg, dis = rednet.receive()
  name, px, py, pz = msg:match("([^,]+),([^,]+),([^,]+),([^,]+)")
  
  if name == os.getComputerLabel() then
    if px == "suck" then
      print(name..":"..px)
      suckChest()
    elseif px == "dots" then
      print(name..":"..px..":"..py)
      makedots(py)
    elseif px == "erase" then
      print(name..":"..px..":"..py)
      erasedots(py)
    elseif px == "faceto" then
      print(name..":"..px..":"..py)
      setFaceTo(py)
    elseif px == "fuel" then
      print(name..":"..px..":"..turtle.getFuelLevel())
      sendFuel()
    else
      gotoxyz(name, px, py, pz)
    end
  end
end
```

### Pocket Computer
I use Pocket Computer for the commander.
<img width="843" alt="スクリーンショット 2025-04-26 22 04 57" src="https://github.com/user-attachments/assets/66cfaeab-e170-4f58-b336-1164968b1a00" />
<img width="657" alt="スクリーンショット 2025-04-26 22 03 59" src="https://github.com/user-attachments/assets/3c0da2c0-390b-4063-b4da-be6d6d260e02" />

#### Commander program for Pocket Computer
[command.lua](https://pastebin.com/vpGQtsRa)

#### setHome
This is my home position.
Please change positions as you like.
"19" is Computer ID of all turtles.
All turtles have the same ID because of copying.

```lua
local function setHome()
  rednet.send(19, "t1,296,67,-244")
  rednet.send(19, "t2,295,67,-244")
  rednet.send(19, "t3,294,67,-244")
  rednet.send(19, "t4,293,67,-244")
  rednet.send(19, "t5,292,67,-244")
  rednet.send(19, "t6,291,67,-244")
  rednet.send(19, "t7,290,67,-244")
  rednet.send(19, "t8,289,67,-244")
  rednet.send(19, "t9,288,67,-244")
  rednet.send(19, "t10,287,67,-244")
end
```
<img width="917" alt="スクリーンショット 2025-04-26 22 13 14" src="https://github.com/user-attachments/assets/0cc57bd5-95ce-4d94-a5c2-3cb885eefa70" />

#### setInit
This is the starting positions to make dots.
```lua
local function setInit()
  rednet.send(19, "t1,296,67,-255")
  rednet.send(19, "t2,296,68,-255")
  rednet.send(19, "t3,296,69,-255")
  rednet.send(19, "t4,296,70,-255")
  rednet.send(19, "t5,296,71,-255")
  rednet.send(19, "t6,296,72,-255")
  rednet.send(19, "t7,296,73,-255")
  rednet.send(19, "t8,296,74,-255")
  rednet.send(19, "t9,296,75,-255")
  rednet.send(19, "t10,296,76,-255")
end
```

#### setDots
This is the main part of this program.
```lua
local function setDots()
  rednet.send(19, "t10,dots,0001111110,0")
  rednet.send(19, "t9,dots,0010001010,0")
  rednet.send(19, "t8,dots,0110111111,0")
  rednet.send(19, "t7,dots,1010001010,0")
  rednet.send(19, "t6,dots,0010111110,0")
  rednet.send(19, "t5,dots,0010001000,0")
  rednet.send(19, "t4,dots,0010001000,0")
  rednet.send(19, "t3,dots,0010001000,0")
  rednet.send(19, "t2,dots,0010010000,0")
  rednet.send(19, "t1,dots,0010100000,0")
end
```
If the turtle gets "1" put a block.
