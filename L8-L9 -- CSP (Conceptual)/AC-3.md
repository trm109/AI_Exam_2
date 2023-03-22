## Algorithm Steps.
1. Convert all binary constraints (ex: A != B) into two arcs (ex: A != B & B != A)
2. Add all arcs to the agenda A.
3. Repeat until agenda A is empty:
	1. Take an arc (Xi, Xj) off the agenda and check it for consistency:
		1. For every value x of Xi:
			1. Is there a value of Xj that is consistent?
				1. Yes -> Keep x.
				2. No -> Remove x.
		2. Remove any inconsistent values from Xi.
	3. If Xi has changed:
		1. Add all arcs of the form (Xk, Xi) to the agenda (excluding (Xj, Xi))
### Example Runthrough:
1. Given:
	1. Variables {A,B,C}
	2. Domain {A={1,2,3}, B={1,2,3}, C={1,2,3}}
	3. Constraints {A > B, B = C}
2. Convert constraints to arcs:
	1. A > B:
		1. {A > B, A < B}
	2. B = C:
		1. {B = C, C = B}
3. Add all Arcs to the Agenda A:
	1. Agenda = {A > B, B < A, B = C, C = B}
4. Begin loop:
	1. Iteration 0 {A > B, B < A, B = C, C = B}:
		1. Take (A > B) out of Agenda.
		2. For every value of A:
			1. Is there a value of B that is consistent:
				1. (2,3) -> Yes, keep
				2. (1) -> No, remove
		3. Since A has changed:
			1. Add any non (A > B) adjacent arcs that ends with A to the Agenda (that isn't already in Agenda):
				1. There are none.
	2. Iteration 1 {B < A, B = C, C = B}:
		1. Take (B < A) out of Agenda.
		2. For every value of B {1,2,3}
			1. Is there a value of A that is consistent:
				1. (1,2) -> Yes, keep
				2. (3) -> No, remove
		4. Since B has changed:
			1. Add any arcs with B as target to the Agenda (that aren't already in it).
				1. Add A > B
	3. Iteration 2 {B = C, C = B, A > B}
		1. Take (B = C) out of Agenda.
		2. For every value of B {1,2}:
			1. Is there any value of C that is consistent:
				1. (1,2) -> Yes, keep
		4. B has not changed.
	4. Iteration 3 {C = B, A > B}:
		1. Take (C = B) out of Agenda.
		2. For every value of C {1,2,3}:
			1. Is there any value of B that is consistent:
				1. (1,2) -> Yes, keep
				2. (3) -> No, remove
		4. Since C has changed:
			1. Add any arc with C as target (that isn't already in Agenda):
				1. B = C
	5. Iteration 4 {A > B, B = C}:
		1. Take (A > B) out of Agenda:
		2. For every value of A {2,3}:
			1. Is there any value of B that is consistent:
				1. (2,3) -> Yes, keep
		4. A has not changed.
	6. Iteration 5 {B = C}:
		1. Take (B = C) out of Agenda:
		2. For every value of B {1,2}:
			1. Is there any value of C that is consistent:
				1. (1,2) -> Yes, keep
		3. B has not changed.
	7. The problem is now Arc Consistent.
## SELECT-UNASSIGNED-VARIABLE

## ORDER-DOMAIN

## Time Complexity.
c = # of arcs.
d = domain size.
The algorithm runs c times, and each c can repeat up to d times at worst case. O(cd):
	Every time the algorithm runs, it has two nested foreachs: O(d^2)
Thus, the algorithm runs at O(cd^3).