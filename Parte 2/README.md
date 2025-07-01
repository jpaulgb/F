# 2. CREATE PORE PRESSURE NODES AND FIXITIES

model BasicBuilder -ndm 2 -ndf 3

set ppNodesInfo [open $outputDir/ppNodesInfo.dat w]
set count 1
set layerNodeCount 0

# loop over soil layers
for {set k 1} {$k <= $numLayers} {incr k 1} {
  # loop in horizontal direction
    for {set i 1} {$i <= $nNodeX} {incr i 2} {
      # loop in vertical direction
        if {$k == 1} {
            set bump 1
        } else {
            set bump 0
        }
        for {set j 1} {$j <= [expr 2*$nElemY($k)+$bump]} {incr j 2} {

            set xCoord  [expr ($i-1)*$sElemX/2]
            set yctr    [expr $j + $layerNodeCount]
            set yCoord  [expr ($yctr-1)*$sElemY($k)/2]
            set nodeNum [expr $i + ($yctr-1)*$nNodeX]

            node $nodeNum  $xCoord  $yCoord

          # output nodal information to data file
            puts $ppNodesInfo "$nodeNum  $xCoord  $yCoord"

          # designate nodes above water table
            set waterHeight [expr $soilThick-$waterTable]
            if {$yCoord>=$waterHeight} {
                set dryNode($count) $nodeNum
                set count [expr $count+1]
            }
        }
    }
    set layerNodeCount [expr $yctr + 1]
}
close $ppNodesInfo
puts "Finished creating all -ndf 3 nodes..."

# define fixities for pore pressure nodes above water table
for {set i 1} {$i < $count} {incr i 1} {
    fix $dryNode($i)  0 0 1
}

# define fixities for pore pressure nodes at base of soil column
fix 1  0 1 0
fix 3  0 1 0
puts "Finished creating all -ndf 3 boundary conditions..."

# define equal degrees of freedom for pore pressure nodes
for {set i 1} {$i <= [expr 3*$nNodeY-2]} {incr i 6} {
    equalDOF $i [expr $i+2]  1 2
}
puts "Finished creating equalDOF for pore pressure nodes..."