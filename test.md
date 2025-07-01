# 1. DEFINE SOIL AND MESH GEOMETRY

set startT  [clock seconds]

set outputDir "C:/Users/Pc_owner2/Desktop/Variacion_Parametros/Borrar/2010"
file mkdir $outputDir

## SOIL GEOMETRY
# thicknesses of soil profile (m)
set soilThick      7.5
# number of soil layers
set numLayers      3
# layer thicknesses
set layerThick(3)  2.5
set layerThick(2)  4.5
set layerThick(1)  0.5
# depth of water table
set waterTable     1.5

# define layer boundaries
set layerBound(1) $layerThick(1)
for {set i 2} {$i <= $numLayers} {incr i 1} {
    set layerBound($i) [expr $layerBound([expr $i-1])+$layerThick($i)]
}

## MESH GEOMETRY
# number of elements in horizontal direction
set nElemX  1
# number of nodes in horizontal direction
set nNodeX  [expr 2*$nElemX+1]
# horizontal element size (m)
set sElemX  2.0

# number of elements in vertical direction for each layer
set nElemY(3)  10
set nElemY(2)  18
set nElemY(1)  2
# total number of elements in vertical direction
set nElemT     30
# vertical element size in each layer
for {set i 1} {$i <=$numLayers} {incr i 1} {
    set sElemY($i) [expr $layerThick($i)/$nElemY($i)]
    puts "size:  $sElemY($i)"
}

# number of nodes in vertical direction
set nNodeY  [expr 2*$nElemT+1]
# total number of nodes
set nNodeT  [expr $nNodeX*$nNodeY]