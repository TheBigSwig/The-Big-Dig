--Functions are defined.

--orient(layerNumber) orients the turtle properly according to the current layer number.
function orient(layerNumber)
  if layerNumber == 1 then
    return true
  else
    while turtle.detectUp() == false do
      if manageFuel(1) == true then
        if turtle.up() == false then
          print "Unable to move up, likely due to entity in the way! Attempting to clear...\n"
          turtle.attackUp()
        end
      else
        print "Unable to properly orient due to lack of fuel!\n"
        return false
      end
    end
    return true  
  end
end

--manageFuel(fuelNeeded) maintains a fuel level sufficient for operational levels, returning true if fuel is sufficient following execution.
function manageFuel(fuelNeeded)
  if turtle.getFuelLevel() < fuelNeeded then
    print("Insufficient fuel to maintain safe operational levels! Attempting to refuel...\n")
    local i = 16
    while turtle.getFuelLevel() < fuelNeeded and i > 0 do
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
  local glowPanels = 0
  local concrete = 0
  for k = 1, 16 do
   local data = turtle.getItemDetail(k)
    if data ~= nil then
      if data.name == "Mekanism:GlowPanel" and data.damage == 15 then
        glowPanels = glowPanels + data.count      
      elseif data.name == "chisel:concrete" and data.damage == 5 then
        concrete = concrete + data.count
      end
    end
  end
  print("Total amount of glow panels: " .. glowPanels)
  print("Total amount of concrete: " .. concrete .. "\n")
  return glowPanels, concrete
end

--enoughResourcesToContinue(layerNumber) checks if there's sufficient resources to continue based off of the current layer number.

function enoughResourcesToContinue(layerNumber)
  local glowPanels, concrete = checkInventory() 
  if layerNumber == 1 then 
    if manageFuel(2) == false then
      print "Unable to continue due to insufficient fuel!\n"
      return false
    elseif concrete < 1 then
      print "Unable to continue due to insufficient concrete!\n"
      return false     
    else
      return true
    end
  elseif layerNumber == 11 then
    if manageFuel(2) == false then
      print "Unable to continue due to insufficient fuel!\n"
      return false
    elseif concrete < 1 then
      print "Unable to continue due to insufficient concrete!\n"
      return false
    elseif glowPanels < 1 then
      print "Unable to continue due to insufficient glow panels!\n"
      return false
    else
      return true
    end
  else
    if manageFuel(1) == false then
      print "Unable to continue due to insufficient fuel!\n"
      return false
    elseif concrete < 1 then
      print "Unable to continue due to insufficient cobblestone!\n"
      return false
    else
      return true
    end
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
  local _, data = turtle.inspectUp()
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

function advanceToNextLayer(layerNumber)
  if layerNumber == 1 then 
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
    if turtle.detectUp() == true then
      print "Error! Movement obstructed!\n"
      return false
    else
      while turtle.up() == false do
        print "Unable to move up, likely due to entity in the way! Attempting to clear...\n"
        turtle.attackUp()
      end
      return true
    end
  else
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
    return true
  end    
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

--completeLayer(layerNumber) mines, replaces, and adds the necessary blocks to complete the layer.

function completeLayer(layerNumber)
  local layerCompleted = true
  local needToMineCeiling = true
  local needToPlaceLight = false
  
  --Making the ceiling block concrete is handled.
  
  local _, dataCeiling = turtle.inspectUp()
  if dataCeiling ~= "No block to inspect" then
    if dataCeiling.name == "chisel:concrete" and dataCeiling.metadata == 5 then
      needToMineCeiling = false
    end
  end
  if needToMineCeiling then
    local success = false
    while success == false and spaceAvailableToMine() do
      if turtle.detectUp() then     
        turtle.digUp()
      end
      if placeBlock("chisel:concrete", 5, "up") then
        success = true
      else
        print "Block placement failed, likely due to entity or fallen block in the way! Attempting to clear...\n"
        turtle.attackUp()
      end
    end
    if not success then 
      layerCompleted = false
    end
  end
 
  --Placing the light is handled.
    if layerNumber == 1 then
      while turtle.down() == false do
        print "Unable to move down, likely due to entity in the way! Attempting to clear...\n"
        turtle.attackDown()
      end
      while placeBlock("Mekanism:GlowPanel", 15, "up") == false do
        print "Block placement failed, likely due to entity in the way! Attempting to clear...\n"
        turtle.attackUp() 
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

--getLayerNumber() retrieves the current layer number from user input.
function getLayerNumber()
  wasSuccessful = true
  print "Please enter the current layer number in the repeating layer pattern (between 1 and 11, 1 for a light layer, 11 for the layer before the subsequent light): "
  local layerNumber = tonumber(io.read())
  if layerNumber == nil then
    print "Invalid input, not a number!\n"
    wasSuccessful = false
  elseif layerNumber > 11 or layerNumber < 1 then
    print "Invalid input, number outside layer number bounds!\n"
    wasSuccessful = false
  end
  return wasSuccessful, layerNumber
end

--Main scripting for turtle.

local canContinue = true
local layersCompleted = 0
local inputSuccessful, currentLayer = getLayerNumber()
if inputSuccessful == false then
  print "Critical error! Operations ceasing!\n"
  return
end
canContinue = orient(currentLayer)

--The starting layer, if unfinished, is completed.
local layerBeforeStaringLayer = currentLayer - 1
if layerBeforeStartingLayer == 0 then
  layerBeforeStartingLayer = 11
end
if enoughResourcesToContinue(layerBeforeStartingLayer) then
  if currentLayer == 1 then
    if turtle.detectUp() == false then
      while turtle.up() == false do
        print "Unable to move up, likely due to entity in the way! Attempting to clear...\n"
        turtle.attackUp()
      end
      canContinue = completeLayer(currentLayer)
    end
  else
    canContinue = completeLayer(currentLayer)
  end
else
  canContinue = false
end

--Subsequent layers are completed.
if canContinue then
  layersCompleted = layersCompleted + 1
  print ("Total layers completed on this run up to this point: " .. layersCompleted .. ".\n")
else
  print "Critical error! Operations ceasing! No layers were completed on this run!"
end
while canContinue == true do
  if enoughResourcesToContinue(currentLayer) then
    if advanceToNextLayer(currentLayer) then
      currentLayer = currentLayer + 1
      if currentLayer == 12 then
        currentLayer = 1
      end
      if completeLayer(currentLayer) then
        layersCompleted = layersCompleted + 1         
        print ("Total layers completed on this run up to this point: " .. layersCompleted .. ".\n")
      else
        canContinue = false
      end
    else
      canContinue = false
    end
  else
    canContinue = false
  end
end
print ("Critical error! Operations ceasing! Total layers completed on this run: " .. layersCompleted .. ".\n")
