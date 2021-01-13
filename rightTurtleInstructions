--Functions are defined.

--orient() orients the turtle properly.
function orient()
  local turnCount = 0
  while turtle.detect() == false do
    if turnCount > 3 then
      print "Error! Unable to find proper orientation!\n"
      return
    end
    turtle.turnRight()
    turnCount = turnCount + 1
  end
end

--manageFuel() maintains a fuel level sufficient for operational levels, returning true if fuel is sufficient following execution.
function manageFuel()
  if turtle.getFuelLevel() < 5 then
    print("Insufficient fuel to maintain safe operational levels! Attempting to refuel...\n")
    local i = 16
    while turtle.getFuelLevel() < 5 and i > 0 do
      turtle.select(i)
      if turtle.refuel(0) == false then
        i = i - 1
      else
        local data = turtle.getItemDetail(i)
        if data.name == "CarpentersBlocks:blockCarpentersSlope" then
          i = i - 1
        else
          turtle.refuel(1)
        end
      end
    end
    if i < 1 then
      print("Error! Out of fuel!\n")
      return false    
    else
      print("Successfully refueled!\n")
      return true
    end
  else
    print("Fuel levels are currently sufficient!\n")
    return true
  end
end

--checkInventory() counts the number of carpenter's slopes and concrete in the turtle's inventory.

function checkInventory()
  local carpentersBlocks = 0
  local concrete = 0
  local cobblestone = 0
  for k = 1, 16 do
   local data = turtle.getItemDetail(k)
    if data ~= nil then
      if data.name == "CarpentersBlocks:blockCarpentersSlope" then
        carpentersBlocks = carpentersBlocks + data.count      
      elseif data.name == "chisel:concrete" and data.damage == 5 then
        concrete = concrete + data.count
      elseif data.name == "minecraft:cobblestone" and data.damage == 0 then
        cobblestone = cobblestone + data.count
      end
    end
  end
  print("Total amount of carpenter's slopes: " .. carpentersBlocks)
  print("Total amount of concrete: " .. concrete)
  print("Total amount of cobblestone: " .. cobblestone .. "\n")
  return carpentersBlocks, concrete, cobblestone
end

--enoughResourcesToContinue() checks if there's sufficient resources to continue.

function enoughResourcesToContinue()
  local carpentersBlocks, concrete, cobblestone = checkInventory() 
  if manageFuel() == false then
    print "Unable to continue due to insufficient fuel!\n"
    return false
  elseif concrete < 1 then
    print "Unable to continue due to insufficient concrete!\n"
    return false     
  elseif carpentersBlocks < 2 then
    print "Unable to continue due to insufficient carpenter's slopes!\n"
    return false
  elseif cobblestone < 2 then
    print "Unable to continue due to insufficient cobblestone!\n"
    return false
  else
    return true
  end
end

--spaceAvailableToMine() determines if there is enough inventory space to mine the block in front of the turtle.

function spaceAvailableToMine()
  local extraSpaceAllocation = 0
  local canDropAlternateItem = false
  local alternateItemName
  local alternateItemMetadata
  local spaceForForm1
  local spaceForForm2
  local _, data = turtle.inspect()
  if data == "No block to inspect" then
    print "Empty block! No need for mining space!\n"
    return true
    
  --Exceptions for fluids (default response is to place act as if fluid is not present).
  
  elseif data.name == "minecraft:flowing_water" then
    print "Fluid! No need for mining space!\n"
    return true
  elseif data.name == "minecraft:flowing_lava" then
    print "Fluid! No need for mining space!\n"
    return true
  elseif data.name == "MineFactoryReloaded:sludge.still" then
    print "Fluid! No need for mining space!\n"
    return true
  elseif data.name == "MineFactoryReloaded:sewage.still" then
    print "Fluid! No need for mining space!\n"
    return true
  elseif data.name == "Additional-Buildcraft-Objects:blockLiquidXP" then
    print "Fluid! No need for mining space!\n"
    return true
  elseif data.name == "BuildCraft|Energy:blockOil" then
    print "Stopping operations! Oil found! It's someone's lucky day!\n"
    return false    
    
  --Exceptions for blocks that change when mined.
 
  elseif data.name == "minecraft:grass" then
    data.name = "minecraft:dirt"
  elseif data.name == "minecraft:stone" then
    data.name = "minecraft:cobblestone"
  elseif data.name == "TSteelworks:Limestone" then
    data.metadata = 1
  elseif data.name == "ProjRed:Exploration:projectred.exploration.stone" then
    data.metadata = 2
    
  --Exceptions for blocks with a chance to drop a different block.
  
  elseif data.name == "minecraft:gravel" then
    canDropAlternateItem = true
    alternateItemName = "minecraft:flint"
    alternateItemMetadata = 0
    spaceForForm1 = false
    spaceForForm2 = false
       
  --Exceptions for ores that give a different item or number of items when mined.   
 
  elseif data.name == "minecraft:coal_ore" then
    data.name = "minecraft:coal"
  elseif data.name == "denseores:block0" and data.metadata == 6 then
    data.name = "minecraft:coal"
    data.metadata = 0
    extraSpaceAllocation = 2
  elseif data.name == "minecraft:redstone_ore" then
    data.name = "minecraft:redstone"
    extraSpaceAllocation = 4
  elseif data.name == "denseores:block0" and data.metadata == 5 then
    data.name = "minecraft:redstone"
    data.metadata = 0
    extraSpaceAllocation = 14
  elseif data.name == "minecraft:lapis_ore" then
    data.name = "minecraft:dye"
    data.metadata = 4
    extraSpaceAllocation = 7
  elseif data.name == "denseores:block0" and data.metadata == 2 then
    data.name = "minecraft:dye"
    data.metadata = 4
    extraSpaceAllocation = 23
  elseif data.name == "minecraft:diamond_ore" then
    data.name = "minecraft:diamond"
  elseif data.name == "denseores:block0" and data.metadata == 3 then  
    data.name = "minecraft:diamond"
    data.metadata = 0
    extraSpaceAllocation = 2  
  elseif data.name == "Railcraft:ore" and data.metadata == 2 then
    data.name = "minecraft:diamond"
    data.metadata = 0  
  elseif data.name == "denseores:block0" and data.metadata == 0 then
    data.name = "minecraft:iron_ore"  
    extraSpaceAllocation = 2
  elseif data.name == "denseores:block0" and data.metadata == 1 then
    data.name = "minecraft:gold_ore"
    data.metadata = 0
    extraSpaceAllocation = 2
  elseif data.name == "minecraft:emerald_ore" then
    data.name = "minecraft:emerald"
  elseif data.name == "denseores:block0" and data.metadata == 4 then
    data.name = "minecraft:emerald"
    data.metadata = 0
    extraSpaceAllocation = 2
  elseif data.name == "Thaumcraft:blockCustomOre" and data.metadata ~= 0 then
    data.name = "Thaumcraft:ItemShard"
    extraSpaceAllocation = 1
    data.metadata = data.metadata - 1
  end
  
  --Space is evaluated.
  
  for i = 1, 16 do
    if turtle.getItemCount(i) == 0 then
      print "Space available to mine!\n"
      return true
    else
      local slotData = turtle.getItemDetail(i) 
      if canDropAlternateItem == false then
        if slotData.name == data.name and slotData.damage == data.metadata and slotData.count < (64 - extraSpaceAllocation) then
          print "Space available to mine!\n"
          return true     
        end
      else
        if slotData.name == data.name and slotData.damage == data.metadata and slotData.count < (64 - extraSpaceAllocation) then 
          spaceForForm1 = true
        elseif slotData.name == alternateItemName and slotData.damage == alternateItemMetadata and slotData.count < (64 - extraSpaceAllocation) then
          spaceForForm2 = true
        end
        if spaceForForm1 and spaceForForm2 then
          print "Space available to mine!\n"
          return true
        end
      end
    end
  end
  print "No space available to mine!\n"
  return false
end

--Brings the turtle to building position in next layer. Returns false if movement is obstructed.

function advanceToNextLayer()
  turtle.turnLeft()
  if turtle.detect() then
    print "Error! Movement obstructed!\n"
    return false
  elseif turtle.attack() then
    print "Error! Entity detected (possibly tunnel bore)! Attempting to clear...\n"
    if turtle.attack() then
      print "Unable to clear entity! Tunnel bore likely reached!\n"
      return false
    else
      print "Entity cleared! Resuming operations!\n"
    end
  end
  turtle.forward()
  turtle.turnRight()
  return true
end  

--placeBlock(name, metadata, direction) places specified block in specified direction, starting at the bottom right of the turtle's inventory.

function placeBlock(name, metadata, direction)
  local itemFound = false
  local i = 16
  while i > 0 and itemFound == false do
    local data = turtle.getItemDetail(i)
    if data ~= nil and data.name == name and data.damage == metadata then
      itemFound = true
      turtle.select(i)
    else
      i = i - 1
    end
  end
  if itemFound then  
    if direction == "up" then
      if not turtle.placeUp() then
        print "Error! Unable to place block!\n"
        return false
      end
    elseif direction == "forward" then
      if not turtle.place() then
        print "Error! Unable to place block!\n"
        return false
      end
    elseif direction == "down" then
      if not turtle.placeDown() then
        print "Error! Unable to place block!\n"
        return false
      end
    else
      print "Error! Improper direction specified!\n"
      return false
    end
  else
    print "Error! Desired block not found!\n"
    return false
  end
  return true
end     

--completeLayer() mines, replaces, and adds the necessary blocks to complete the layer.

function completeLayer()
  local layerCompleted = true
  local needToMineFront = true
  local needToPlaceTop = false
  local needToPlaceBottom = false
  
  --Making the front block concrete is handled.
  
  local _, dataFront = turtle.inspect()
  if dataFront ~= "No block to inspect" then
    if dataFront.name == "chisel:concrete" and dataFront.metadata == 5 then
      needToMineFront = false
    end
  end
  if needToMineFront then
    local success = false
    while success == false and spaceAvailableToMine() do
      if turtle.detect() then     
        turtle.dig()
      end
      if placeBlock("chisel:concrete", 5, "forward") then
        success = true
      else
        print "Block placement failed, likely due to entity or fallen block in the way! Attempting to clear...\n"
        turtle.attack()
      end
    end
    if not success then 
      layerCompleted = false
    end
  end
 
  --Placing the top carpenter's slope is handled.
  
  local _, dataTop = turtle.inspectUp()
  if dataTop ~= "No block to inspect" then
    if dataTop.name ~= "CarpentersBlocks:blockCarpentersSlope" or not (dataTop.metadata == 0 or dataTop.metadata == 5) then
      if turtle.detectUp() then
        print "Warning! Obstruction detected where carpenter's slope should be!\n"
        layerCompleted = false
      end
    end
  else
    while turtle.up() == false do
      print "Unable to move up, likely due to entity in the way! Attempting to clear...\n"
      turtle.attackUp()
    end
    _, dataTop = turtle.inspectUp()
    if dataTop == "No block to inspect" then
      while placeBlock("minecraft:cobblestone", 0, "up") == false do
        print "Block placement failed, likely due to entity in the way! Attempting to clear...\n"
        turtle.attackUp()
      end
    else
      if turtle.detectUp() == false then
        if dataTop.name == "BuildCraft:Energy:blockOil" then
          print "Stopping operations! Oil found! It's someone's lucky day!\n"
          layerCompleted = false
        else
          while placeBlock("minecraft:cobblestone", 0, "up") == false do
            print "Block placement failed, likely due to entity in the way! Attempting to clear...\n"
            turtle.attackUp()
          end
        end
      end
    end
    while turtle.down() == false do
      print "Unable to move down, likely due to entity in the way! Attempting to clear...\n"
      turtle.attackDown()
    end
    while placeBlock("CarpentersBlocks:blockCarpentersSlope", 0, "up") == false do
      print "Block placement failed, likely due to entity in the way! Attempting to clear...\n"
      turtle.attackUp() 
    end
  end    
  
  --Placing the bottom carpenter's slope is handled.
  
  local _, dataBottom = turtle.inspectDown()         
  if dataBottom ~= "No block to inspect" then
    if dataBottom.name ~= "CarpentersBlocks:blockCarpentersSlope" or not (dataBottom.metadata == 0 or dataBottom.metadata == 5) then
      if turtle.detectDown() then
        print "Warning! Obstruction detected where carpenter's slope should be!\n"    
        layerCompleted = false
      end
    end
  else
    while turtle.down() == false do
      print "Unable to move down, likely due to entity in the way! Attempting to clear...\n"
      turtle.attackDown()
    end
    _, dataBottom = turtle.inspectDown()
    if dataBottom == "No block to inspect!" then
      while placeBlock("minecraft:cobblestone", 0, "down") == false do
        print "Block placement failed, likely due to entity in the way! Attempting to clear...\n"
        turtle.attackDown()
      end
    else
      if turtle.detectDown() == false then
        if dataBottom.name == "BuildCraft:Energy:blockOil" then
          print "Stopping operations! Oil found! It's someone's lucky day!\n"
          layerCompleted = false
        else
          while placeBlock("minecraft:cobblestone", 0, "down") == false do
            print "Block placement failed, likely due to entity in the way! Attempting to clear...\n"
            turtle.attackDown()
          end
        end
      end
    end
    while turtle.up() == false do
      print "Unable to move up, likely due to entity in the way! Attempting to clear...\n"
      turtle.attackUp()
    end  
    while placeBlock("CarpentersBlocks:blockCarpentersSlope", 0, "down") == false do
      print "Block placement failed, likely due to entity in the way! Attempting to clear...\n"
      turtle.attackDown()
    end
  end
  
  --Return is based on if the layer was successfully completed.
  if layerCompleted then
    print "Layer successfully completed!\n"   
    return true
  else
    print "Layer was not successfully completed!\n"
    return false
  end
end  

--Main scripting for turtle.

local canContinue = true
local layersCompleted = 0
canContinue = orient()
if enoughResourcesToContinue() then
  canContinue = completeLayer()
else
  canContinue = false
end
if canContinue then
  layersCompleted = layersCompleted + 1
  print ("Total layers completed on this run up to this point: " .. layersCompleted .. ".\n")
else
  print "Critical error! Operations ceasing! No layers were completed on this run!"
end
while canContinue == true do
  if enoughResourcesToContinue() and advanceToNextLayer() and completeLayer() then
    layersCompleted = layersCompleted + 1         
    print ("Total layers completed on this run up to this point: " .. layersCompleted .. ".\n")
  else
    canContinue = false
    print ("Critical error! Operations ceasing! Total layers completed on this run: " .. layersCompleted .. ".\n")
  end
end
