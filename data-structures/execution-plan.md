# Execution Plan Data Structure

1. An Execution plan consists of execution segments.
2. An execution segment consists of a flat array of execution steps.
3. Branching into multiple segments is achieved by an explicit execution step which maintains primary segment output which branching n-fold.
4. The branching step contains the side segments in the header - its data payload remains unaffected.
5. Visualisation and management tools are needed to view / create / update.