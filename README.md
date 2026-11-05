# Cyberpunk 2077 — Breach Protocol Solver

A Python notebook that extracts a **Breach Protocol** puzzle from a screenshot of *Cyberpunk 2077*, recognizes the hexadecimal symbols, and searches for a valid path through the code matrix.

The project combines **OpenCV image processing**, **connected-component analysis**, **DBSCAN spatial clustering**, **Jaccard-based template matching**, **sequence merging**, and **recursive depth-first search (DFS) with backtracking**.

## The problem

In Breach Protocol, the player selects hexadecimal symbols from a code matrix to complete required sequences within a limited buffer. The first selection must be made from the top row; subsequent selections alternate between columns and rows, and a cell cannot be selected twice.

The solver aims to identify a **high-value, feasible sequence of moves**, taking these restrictions into account. It is a notebook-based demonstration, **not** a real-time game automation tool.

## How it works

1. **Image processing:** Inspect color channels, detect the bright `CODE MATRIX` header, crop the screenshot, and isolate the code matrix and required-sequences regions using local median-difference thresholding and connected-component analysis.
2. **Symbol recognition:** Extract and normalize binary symbols to 50 × 50 pixels. Classify them against six reference templates (`55`, `1C`, `BD`, `E9`, `7A`, and `FF`) using Jaccard similarity. Use DBSCAN and spatial ordering to reconstruct the matrix and target sequences.
3. **Sequence optimization:** Merge targets using suffix–prefix overlaps and containment checks. Generate candidate combinations that fit within the buffer, score each candidate against **all** original target sequences it satisfies, and prioritize higher-scoring candidates.
4. **Pathfinding:** Use recursive DFS with backtracking to search for a valid path. The target may appear at the **end** of a longer selected sequence—for example, `[1, 2, 3, 4]` satisfies `[2, 3, 4]`.
5. **Results:** Show the selected sequence, buffer usage, a numbered matrix representation, and a visual comparison of the original screenshot with the selected path.

## Output
s
The notebook produces:

- A reconstructed **code matrix** (NumPy array).
- The recognized **required sequences**.
- A prioritized list of candidate symbol sequences.
- A valid selection path, when one is found, with buffer usage and step numbers.
- A side-by-side display of the original screenshot and the reconstructed solution path.

Intermediate visualizations document how the image is processed and the symbols are classified.

## Assumptions and limitations

- **Heuristic optimization:** Candidate scoring counts all targets contained in each candidate, but greedy sequence merging does not enumerate every possible supersequence. The first feasible high-scoring candidate is not guaranteed to be the globally highest-reward path.
- **Reward model:** Target sequences are assigned linearly increasing values from top to bottom (1, 2, 3, ...). These are assumed priorities, not necessarily the game's actual reward amounts.
- **Image sensitivity:** Some thresholds, morphological kernels, component filters, and clustering parameters are calibrated to the example image. Other resolutions, camera angles, or interface appearances may need adjustments.
- **Template dependence:** Recognition uses six precomputed binary templates rather than a trained machine-learning model.
- **Search complexity:** DFS may become expensive for larger boards and buffers.
- **Validation:** The notebook demonstrates the supplied example; performance across a broad screenshot dataset has not been established.

## Possible improvements

- Make region detection and preprocessing more robust across photos.
- Add automated tests and multiple annotated examples.
- Evaluate complete DFS paths by reward rather than relying solely on merged target candidates.
- Add pruning or memoization to reduce search time.

## Project purpose

This is a personal programming and computer-vision portfolio project demonstrating how image processing, symbol recognition, combinatorial reasoning, and backtracking can be integrated into a single solution pipeline.
