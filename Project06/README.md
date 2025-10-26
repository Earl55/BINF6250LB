# Introduction
This project focuses on phylogenetics of HIV-1 reverse transcriptase of several organisms to create a phylo tree. Tjhis project shows relationships in genetic trees, origins. Does two organisms relate to each other by a common connection? A Smith Waterman walk matrix is used tp score the relationships and from there. Build another matrix for the distance. Evemtually doing Neighbor Joining to help the main point is the tree.

# Pseudocode
Put pseudocode in this box:

```
### **Main Program**

       
BEGIN PROGRAM

Define the input file
  fasta_file = "lafayette_SARS_RT.fasta"

  TRY:
    Read the sequence data
    sequences = read_fasta(fasta_file)

    Calculate scores and build the distance matrix
    distance_matrix, sequence_labels = build_distance_matrix(sequences)

    Construct the tree using the Neighbor-Joining algorithm
    newick_tree_string = neighbor_joining(distance_matrix, sequence_labels)

    Visualize the resulting tree
    plot_tree(newick_tree_string)

    CATCH errors (e.g., file not found, library import error):
    PRINT a descriptive error message
  END TRY

END PROGRAM


#### **1. read_fasta**

*To parse a FASTA file and load sequence data into a usable data structure.*

       
read_fasta(filename):
  Input: The path to a FASTA file.
  Output: A dictionary mapping sequence IDs to their full sequences.

  INITIALIZE an empty dictionary `sequences`

  OPEN the `filename` for reading
  FOR EACH line in the file:
    IF the line is a header (starts with '>'):
      EXTRACT the sequence ID and prepare a new entry in the dictionary
    ELSE:
      APPEND the sequence data to the current entry

  RETURN `sequences`
END FUNCTION


#### **2. smith_waterman_score**

*To calculate the maximum raw score for the optimal local alignment between two sequences.*

       
smith_waterman_score(sequence1, sequence2, scoring_parameters):
  Input: Two sequence strings and scoring parameters.
  Output: A single number representing the maximum score.

  INITIALIZE a scoring matrix with zeros, sized based on sequence lengths
  INITIALIZE `max_score` to 0.0

  FOR EACH cell in the scoring matrix:
    CALCULATE potential scores from diagonal, up, and left moves, considering matches, mismatches, and gaps

    SET the cell's value to the maximum of the potential scores and zero

    UPDATE `max_score` if the cell's value is greater

  RETURN `max_score`
END FUNCTION


#### **3. build_distance_matrix**

*To compute a symmetric distance matrix for all pairs of sequences.*

       
build_distance_matrix(sequences):
  Input: A dictionary of sequences.
  Output: A 2D array (distance matrix) and a list of sequence IDs.

  EXTRACT sequence IDs and strings into ordered lists
  INITIALIZE a square matrix `dist_matrix` with zeros

  For normalization, first calculate the self-alignment score for each sequence
  FOR EACH sequence:
    CALCULATE its self-alignment score using smith_waterman_score and STORE it

  Fill the distance matrix
  FOR EACH unique pair of sequences (sequence_A, sequence_B):
    Get the raw Smith-Waterman score between the pair
    `raw_score` = smith_waterman_score(sequence_A, sequence_B)

    Normalize the score to a similarity value between 0 and 1
    CALCULATE `normalized_similarity` using the raw score and the pre-calculated self-scores of sequence_A and sequence_B

    Convert similarity to distance
    `distance` = 1.0 - `normalized_similarity`

    Populate the distance matrix symmetrically with the calculated distance
    SET the matrix entries for (A,B) and (B,A) to the `distance`

  RETURN `dist_matrix` and the list of sequence IDs
END FUNCTION


#### **4. neighbor_joining**

*To iteratively build a phylogenetic tree in Newick format from a distance matrix.*

       
neighbor_joining(distance_matrix, labels):
  Input: A distance matrix and a list of corresponding labels.
  Output: A single string representing the tree in Newick format.

  INITIALIZE a list of `active_nodes` based on the input `labels`
  INITIALIZE a working copy of the `distance_matrix`

  WHILE more than two nodes remain in `active_nodes`:
    Calculate the Q-matrix based on the current distance matrix
    CALCULATE the Q-matrix to find the best pair to join

    Find the pair of nodes with the minimum Q-value
    IDENTIFY the two nodes that are the best candidates for joining

    Calculate the branch lengths for the new internal node
    CALCULATE the lengths of the two new branches connecting the pair to their common ancestor

    Create a new internal node representing the joined pair
    FORMULATE a Newick-formatted string for the new node, including its children and branch lengths

    Calculate the distances from this new node to all other active nodes
    CALCULATE a new set of distances for the merged node

    Reduce the distance matrix and labels list
    UPDATE the distance matrix by removing the joined pair and adding the new node
    UPDATE the list of active nodes similarly

    Join the last two remaining nodes to complete the tree
    CONSTRUCT the final Newick string for the root of the tree

  RETURN the completed Newick tree string
END FUNCTION


#### **5. plot_tree**

*To visualize a Newick-formatted tree string and save it as an image file.*

       
plot_tree(newick_tree, filename):
  Input: A Newick tree string and an output filename.
  Output: An image file of the tree.

  TRY:
    Use a library (e.g., Bio.Phylo) to parse the `newick_tree` string into a `tree` object
    `tree` = parse_newick_string(`newick_tree`)

    Use a plotting library (e.g., Matplotlib) to create a figure and axes
    CREATE a figure and axes for plotting

    Draw the `tree` object onto the axes, with options to hide unnecessary labels or borders
    RENDER the tree object on the plot

    Save the figure to the specified `filename`
    SAVE the plot to `filename`

    PRINT a success message

  CATCH any errors (e.g., library not found):
    PRINT a descriptive error message
END FUNCTION

```

# Successes
Realizing that we can optimize some of the functions

# Struggles
The ete3 package was the major issue of this assignment. Before I chose to ask the class group chat I tried sp many ways to get this installed. Tiaage was able to realize using the bioconda env

# Personal Reflections
## Group Leader
I was so nervous to be the group the leader because I thought i was not ready. I read the background and tried to understand the background more. I went through the old modules and PRs to understand if I was
setting up the repo correctly. I  had major issues with the installing of the ete3 package. After a day or two of trying this find ways to install this. By switching python enviroments and even trying to look at the version. I was able with the help of Tiage simplfying the read fasta file function. Working with Tiaggge was very collaborative and constructive.

## Other member

### Tiange Feng

During this assignment, I consulted Jackie and Chris regarding a `ModuleNotFoundError: No module named 'cgi'` issue when trying to import the `ete3` package. They suggested using an alternative module to circumvent this problem, which I thought was an excellent approach.

While most of this project covered concepts we had already encountered in previous courses, Little and I found that this specific assignment required meticulous attention to the 'hand-offs'—the exact input and output connections—between the different functions.

# Generative AI Appendix
I used Google Gemini to organize the original version of my pseudocode because it was quite lengthy and difficult to understand in its initial form.  -Tiange Feng
