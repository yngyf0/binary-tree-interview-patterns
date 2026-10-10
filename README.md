# binary tree interview questions: the 13 shapes that cover almost every tree problem, with patterns, complexity answers, and the follow-ups interviewers actually ask

Tree questions have a reputation problem. People expect a 400-problem grind and get ambushed by the same dozen questions wearing different costumes. Invert a tree becomes "mirror this tree." Validate BST becomes "is this a valid search tree, but here it's a string of digits." Path sum becomes "sum of all root-to-leaf paths."

If you can name the shape of a problem within two minutes of reading it, the implementation is usually 15 lines. That's the skill these questions are testing. Below is the map — the questions that actually recur, the pattern each one belongs to, and the complexity numbers you should be able to say out loud without hesitating.

## What tree questions are really screening for

Interviewers aren't checking whether you've memorized 40 solutions. They're checking four things:

1. **Can you write recursive code without freezing?** Base case, recursive call, combine. If you fumble the base case under pressure, that's the signal.
2. **Can you choose DFS or BFS for a reason?** Not "I'll use recursion because it's shorter," but "I need level-by-level output, so it's BFS."
3. **Do you know what your solution costs?** O(n) time is easy. Space is where people get quiet — and it depends on tree shape, which is a great follow-up hook.
4. **Do you handle the boring edges?** Empty tree. Single node. Duplicate values. A depth of 10,000 nodes that blows the recursion stack.

Everything else is decoration.

## The binary tree interview questions that actually come up

Grouped by the pattern they belong to, because that's how you should study them:

| Question | Pattern | Answer shape | Time / Space |
| --- | --- | --- | --- |
| Preorder, inorder, postorder traversal | DFS on tree | Recursive, or explicit stack | O(n) / O(h) |
| Iterative inorder (no recursion allowed) | DFS with stack | Push-left loop | O(n) / O(h) |
| Level order, zigzag, right side view | BFS on tree | Queue + per-level size loop | O(n) / O(w) |
| Validate BST | Range-passing recursion, or inorder with a `prev` pointer | Two bounds travelling down | O(n) / O(h) |
| Invert binary tree | DFS | Swap children, recurse | O(n) / O(h) |
| Same tree, symmetric tree | DFS on two roots | Compare mirrored nodes | O(n) / O(h) |
| Maximum depth, minimum depth, balanced check | DFS returning height | Height bubbling up | O(n) / O(h) |
| Diameter of binary tree | Divide and conquer | Return height, track global max | O(n) / O(h) |
| Binary tree maximum path sum | Tree DP | Return best one-sided gain, track best total | O(n) / O(h) |
| Lowest common ancestor (general tree and BST) | Recursive split | Return first node where targets diverge | O(n) general, O(h) BST |
| Path sum I and II, print all root-to-leaf paths | DFS with backtracking | Running sum, undo on return | O(n) / O(h) |
| Serialize and deserialize | Preorder + null markers | Encode with placeholders, rebuild with an index | O(n) / O(n) |
| Construct from preorder + inorder | Divide and conquer | Hashmap for index lookup | O(n) / O(n) |
| Kth smallest in BST / BST iterator | Inorder | Counter, or a stack you pause | O(h) / O(n) |

Two names repeat as the most-cited favourites in prep circles: **Validate BST** and **Lowest Common Ancestor**. If you only had two hours before a screen, those are the two. Add level-order traversal as a close third, because it's the pattern most people reach for too late.

## Traversal is the answer to more questions than it looks like

Every tree question you'll meet is some traversal plus a small tweak.

Preorder visits the node before its subtrees — useful when the parent's state has to be known first, like passing minimum and maximum bounds down in BST validation. Inorder visits left, node, right, and on a BST that produces sorted output, which is the entire trick behind "kth smallest," "BST iterator," and validating a BST. Postorder visits children before the parent, which is what you want whenever a node's answer depends on its children's answers — heights, diameters, subtree sums.

The subtlety interviewers probe: **the recursion depth equals the height of the tree, not the number of nodes.** A balanced tree with a million nodes has height around 20, so the stack is trivial. A skewed tree shaped like a linked list has height 1,000,000 — which will crash in most languages before you finish typing. That distinction is worth saying out loud before you're asked.

If you want the iterative versions:

- **Preorder** — stack, push right child before left.
- **Inorder** — stack, push left spine, pop, go right.
- **Postorder** — two stacks, or one stack with a `lastVisited` pointer.
- **Level order** — queue, and record `len(queue)` at the top of each round to know where the level ends.

Morris traversal gets you O(1) space by temporarily rewiring right pointers. It's rarely required, but mentioning it as an option when asked "can you do this without extra space?" lands well.

## The BST validation trap

The mistake is checking each node locally: left child smaller, right child bigger, recurse. This passes a tree that is not a BST — a small value buried deep in the right subtree satisfies its immediate parent while violating the root's constraint.

Two correct approaches. Pass down `(min, max)` bounds, narrowing them as you descend — the root gets `(-inf, +inf)`, its left child gets `(-inf, root.val)`, its right child gets `(root.val, +inf)`. Or run an inorder traversal and check that each value is strictly greater than the previously visited one.

Worth knowing: for an inorder solution you can avoid an extra array by keeping a single `prev` pointer. Interviewers often ask you to drop the array, and that's the expected answer.

One more detail: whether duplicates are allowed is a decision, not a fact. Ask.

## Where dynamic programming quietly walks in

Once a problem talks about "the longest path between any two nodes" or "the best path through the tree," the return value and the tracked answer stop being the same thing.

**Diameter** is the classic. At each node, you return the height of the subtree, but you also track `leftHeight + rightHeight` as a candidate answer. The path doesn't have to pass through the root.

**Maximum path sum** is the same shape with a twist: the value you return is the best gain from a single side (or zero, if both sides are negative), while the tracked answer is `node.val + leftGain + rightGain`. The reason you can't return the full path is that a path going up to the parent can only enter this subtree from one side.

Same skeleton, different bookkeeping. Once you see one, you see both.

## The complexity answers to have ready

Say these without pausing:

- **Traversal of any kind** is O(n) time — you touch each node once.
- **DFS space** is O(h) for the recursion stack, which is O(log n) for a balanced tree and O(n) for a skewed one.
- **BFS space** is O(w), the maximum width. In a perfect binary tree the last level holds about n/2 nodes, so BFS on a wide tree is O(n) space even though the tree is shallow.
- **Search, insert, delete in a BST** are O(h) — O(log n) balanced, O(n) skewed. That's the whole argument for AVL and red-black trees, and interviewers sometimes ask you to connect those dots.
- **Building a tree from preorder and inorder** is O(n) with a hashmap for index lookups, O(n log n) or worse without one.
- **Serialization** is O(n) time and O(n) space for the string; the recursion itself is O(h).

If a question says "n nodes" and you answer "O(log n) space" without qualifying shape, expect the follow-up.

## The follow-ups that actually decide the round

Getting the answer right is table stakes. These are the questions that follow:

- **"The tree is 100,000 nodes deep. Recursion or not?"** Iterative traversal with an explicit stack, or Morris if space is tight.
- **"What if each node also had a parent pointer?"** Lowest common ancestor becomes an intersection-of-two-linked-lists problem — no recursion needed.
- **"How would you serialize with less overhead?"** Compact encodings, or store structure separately from values.
- **"Can you do it in one pass instead of two?"** Usually yes, by tracking a secondary value during the same recursion — the diameter trick.
- **"What changes if duplicates are allowed in a BST?"** Tighten the bound to strict on one side and non-strict on the other, and say which side you chose.
- **"What if the tree is stored in an array, heap-style?"** Children of index `i` live at `2i+1` and `2i+2` — that's also the neat way to check whether a tree is complete without a queue.

Most candidates who "solve the problem but don't pass" lose the round here, not on the first answer.

## A three-week plan for tree questions specifically

Tree prep compresses well because the question pool is small.

**Week 1 — traversal fluency.** Write preorder, inorder, postorder, and level order recursively and iteratively, in the language you'll interview in. Then solve the traversal variations: zigzag, right side view, and print-all-paths. The goal isn't solving — it's typing the templates from memory without looking.

**Week 2 — structure and BST.** Validate BST, invert, same tree, symmetric, balanced, max depth, minimum depth, count nodes, count leaves. Once you're warm, write the iterative inorder and the BST iterator. This week is where most of the recurring questions live.

**Week 3 — path problems and tree DP.** Diameter, maximum path sum, path sum I and II, LCA both variants, serialize and deserialize, construct from traversals. These are the ones that separate candidates, and they all build on week 1.

Then do timed sets. Not because speed is the point, but because a real interview runs 40 minutes and you need to feel the clock.

## Where a pattern-based course fits this

If you'd rather not assemble your own curriculum from scattered problem lists, AlgoMonster takes the same pattern-first view of tree problems, which is why it comes up in this conversation. Its own decision flowchart routes tree problems by category: is it a tree, are you counting or generating many trees, do you need a level-order answer. That's what splits the tree section into **DFS on Tree**, **BFS on Tree**, and **Divide and Conquer / Tree DP** — the same three buckets this article uses, which is not a coincidence. The three-way split is how tree questions actually behave.

A few things worth knowing before you buy anything:

- **Format is text and diagrams, no videos.** Lessons, templates, and interactive diagrams. If you need someone talking through a solution, this isn't it.
- **The problem count is smaller than LeetCode's, by design.** The pitch is that a mapped set of problems tied to explicit patterns beats a huge unsorted bank. Supplement with company-tagged practice if you want volume.
- **System design and behavioral coverage are introductory.** For L5+ loops where system design is a graded round, you'll need a dedicated resource.
- **There's a free tier with usable practice content**, plus a 3-day free trial on the subscribe page — enough to test whether the reading-and-diagrams format works for you before paying.

### AlgoMonster plans, current as listed on the site

Prices move with promotions, so treat the checkout page as the source of truth. The subscribe page has shown $99/year and $189 lifetime with a 50% sale applied to listing prices of $198 and $378; during deeper sales it has shown $69.99/year and $89 lifetime.

| Plan | What you get | Price | Billing | Get it |
| --- | --- | --- | --- | --- |
| Free | Limited access to a subset of patterns and problems; free practice content to test the format | $0 | — | [ Try the free practice content](https://algo.monster?fpr=li69) |
| Pro — Yearly | All patterns, lessons and problems unlocked; unlimited AI assistant and Speedrun; company questions updated monthly; 500+ lessons across 6 courses, 1,000+ illustrations, 3,000+ editorials; bonus Foundation and Build-an-LLM courses | ~$99/year listed on the subscribe page (~$8.25/mo billed annually) against a $198 list price | 12 months, renews annually, cancel or upgrade anytime | [ See the current yearly price](https://algo.monster?fpr=li69) |
| Pro — Lifetime | Everything in the yearly plan, lifetime updates, and an internal referral to a top tech company for qualified candidates | $189 one-time against a $378 list price; deeper sales have shown $89 | One-time payment | [ Check the lifetime price](https://algo.monster?fpr=li69) |

Two honest notes. First, there's no monthly billing — it's annual or lifetime. Second, coupon codes circulate on discount sites (SCHELLEY, FAANG20, and others), and I couldn't independently verify that any of them still apply at checkout; the sale discount already appears baked into the listed price, so test a code if you have one and don't count on it.

The break-even math is simple: the lifetime plan costs roughly what two years of the yearly plan does, and it makes sense if you expect to interview more than once in the next few years. If you have a screen in six weeks and then you're done for a while, yearly is the cheaper bet.

## Bottom line

Tree questions are one of the few interview topics where a genuinely bounded study plan exists. Thirteen shapes, three patterns — DFS, BFS, divide and conquer — and a handful of complexity sentences you should be able to recite. The parts that trip people up aren't the algorithms; they're the edges, the space complexity on a skewed tree, and the follow-up questions nobody rehearsed.

Learn the traversal templates until they're muscle memory, then spend your remaining time on LCA, validate BST, and the two tree-DP problems. If you want a structured path instead of building one from scratch, [👉 start with the free content on the AlgoMonster site](https://algo.monster?fpr=li69) and see whether the format clicks — the free tier plus the 3-day trial exists precisely so you can find out before spending anything.
