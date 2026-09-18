---
title: Bend 2 and the Vibe-Coding Trap
link: https://blog.liampwll.com/posts/bend_vibe_coding/
source: hnrss-org-frontpage
published: 2026-09-18T12:03:55Z
updated: 2026-09-18T12:03:55Z
first_seen: 2026-09-18T16:19:55.454833534Z
authors:
- LiamPowell
summary: 'Article URL: https://blog.liampwll.com/posts/bend_vibe_coding/ Comments URL: https://news.ycombinator.com/item?id=49753179 Points: 275 # Comments: 202'
content: extracted
html: 2026-09-18-bend-2-and-the-vibe-coding-trap.html
---

*Bend just serves as a useful example of my general point regarding vibe-coding as it is recent, high-profile, and has aspects that make it easy to use as an example. I don’t know anything about the author’s history with designing languages or if they actually did consider the tradeoffs below and made what I think is a poor choice. Feel free to replace “the author” below with “a hypothetical author who could have created the same thing”.*

*I obviously do not like the design decisions made in Bend and I wanted to present those, however that’s been overly conflated with the main point that I’m trying to make below. I don’t want to edit this now and make it seem like any existing comments are being overly harsh so I feel that the best solution is explaining what my mental model of the article looked like while writing it instead.*

*[Here](https://news.ycombinator.com/item?id=49753898) is a comment by the author of Bend.*

[Bend 2](https://bend-lang.com/) is being pitched as a language for the AI coding era: humans write “laws”, AI writes implementations and proofs, and the compiler checks that the proofs are sound. That all sounds quite impressive and I can see why someone would want a language that does that. There are actually a few major problems with this idea; however, that’s not what this article about. Instead I want to talk about how the Bend itself seems to have fallen in to a common trap with vibe-coding that I don’t see mentioned much.

Let’s start with a baseline of what Bend requires the developer to write for its demo on the home page:

[https://github.com/bendlang/bend/blob/main/demos/app\_win\_is\_bug\_2d/LAWS.bend](https://github.com/bendlang/bend/blob/main/demos/app_win_is_bug_2d/LAWS.bend)

I won’t reproduce it here because the code isn’t too important. What is important for this article is that it’s quite a bit of code. It’s 58 lines of code just to state that the player can never touch the flag or win the game. There’s also other problems in that the LLM can redefine the `Game` subprograms to do anything; however, that’s once again not the point of the article.

Next up lets look at what the LLM writing the code for this program needs to write in order to prove the “laws”:

[https://github.com/bendlang/bend/blob/main/demos/app\_win\_is\_bug\_2d/PROOF.bend](https://github.com/bendlang/bend/blob/main/demos/app_win_is_bug_2d/PROOF.bend)

That’s a lot. 442 lines of code to prove those simple properties.

So what’s the problem I have with this? Why am I calling it a vibe-coding trap?

The problem is that vibe coding makes it possible to build a substantial solution before learning enough about the problem to recognise that a much better solution exists. A developer can produce an entire language and compiler while missing an approach that an introductory survey of the field would have put directly in front of them.

The field in question is formal verification. It’s notable that those two words appear nowhere on Bend’s webpage or in its codebase. The developer has built an entire language around a field seemingly without realising that said field exists.

To clearly demonstrate why this is a problem, let’s recreate the same program that Bend uses as a demo in SPARK, an open source language and compiler for formal verification. To be fair to Bend, I completely vibe-coded this, I just told a LLM to recreate the demo in SPARK with no further guidance:

```ada
package Game with SPARK_Mode is
   subtype Column is Integer range 0 .. 11;
   subtype Row is Integer range 0 .. 7;
   type State is record
      X : Column;
      Y : Row;
      Won : Boolean;
   end record;
   Start : constant State := (8, 5, False);

   function Wall (X : Column; Y : Row) return Boolean is
     (((X = 3 or X = 11) and Y <= 3)
      or ((Y = 3 or Y = 7) and X <= 3));
   function Cell (X : Column; Y : Row) return Character is
     (if Wall (X, Y) then '#' elsif X = 1 and Y = 1 then 'F' else '.');

   --  Inductive invariant: outside the sealed room, off walls, not won.
   function Safe (G : State) return Boolean is
     ((G.X > 2 or G.Y > 2) and not Wall (G.X, G.Y) and not G.Won)
     with Ghost;
   procedure Step (G : in out State; Key : Character)
     with Post => (if Safe (G'Old) then Safe (G));

   --  Both Bend laws, including the actual cell drawn by the terminal.
   function Replay (Keys : String) return State
     with Post => not Replay'Result.Won
       and Cell (Replay'Result.X, Replay'Result.Y) /= 'F';
end Game;

------------------------------

package body Game with SPARK_Mode is
   procedure Step (G : in out State; Key : Character) is
      X : Column := G.X;
      Y : Row := G.Y;
   begin
      case Key is
         when 'w' => Y := (Y - 1) mod 8;
         when 's' => Y := (Y + 1) mod 8;
         when 'a' => X := (X - 1) mod 12;
         when 'd' => X := (X + 1) mod 12;
         when others => return;
      end case;
      if not Wall (X, Y) then
         G := (X, Y, G.Won or Cell (X, Y) = 'F');
      end if;
   end Step;

   function Replay (Keys : String) return State is
      G : State := Start;
   begin
      for Key of Keys loop
         pragma Loop_Invariant (Safe (G));
         Step (G, Key);
      end loop;
      return G;
   end Replay;
end Game;

------------------------------

with Ada.Text_IO; use Ada.Text_IO;
with Game; use Game;

procedure Main is
   G : State := Start;
begin
   Put_Line ("Winning is impossible. WASD + Enter to move; q + Enter to quit.");
   loop
      for Y in Row loop
         for X in Column loop
            Put (if X = G.X and Y = G.Y then 'P' else Cell (X, Y));
         end loop;
         New_Line;
      end loop;
      Put_Line (if G.Won then "WON (this should be unreachable)" else "still not won");
      exit when End_Of_File;
      declare
         Keys : constant String := Get_Line;
      begin
         exit when Keys = "q";
         for Key of Keys loop
            Step (G, Key);
         end loop;
      end;
   end loop;
end Main;
```

So now we have the same laws defined as Bend, what’s the point I’m trying to make here?

Where this differs from Bend is that what we have supplied here is everything required to prove the correctness of the program, without having a LLM waste time and tokens on building up a 442 line proof from first principles. We can run GNATprove and get:

```
Success: all checks proved (12 checks).
```

The author of Bend has completely missed that this is the current standard in the field of formal verification, if they even know that this field exists at all. They have instead come up with this whole system requiring verbose specifications and even more verbose proofs. A little research before vibe-coding an entire language and compiler could have substantially improved the result because the author would have known what to ask for.

This example matters beyond Bend, vibe-coding makes it makes it far too easy to implement a design that’s horribly broken or decades behind the current state of the art because you can immediately get a result without ever having to do any research. If you ask a LLM for a language where it’s possible to prove that a function is formally correct by building up a proof from basic principles then it will happily do so, it will never stop to suggest to you that computers can already build complex proofs without the need for a LLM and eliminate 99% of the work. It will never tell you that what you’re building already mostly exists as work that you can build on.
