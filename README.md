# merge-quirk

An attempt to merge merge-src into merge-dst shows a conflict due to different changes that occured on both branches, but resolving the conflict in favor of the merge-src version would result in code disappearing.

I am having a hard time wrapping my head around what is happening.

Can anyone explain this to me ?

A sample session follows. Note that if the origin/merge-src version is accepted the `if thisLineMightDisapper(42))` is no longer present.
```
geretz$ git clone https://github.com/geretz/merge-quirk.git
Cloning into 'merge-quirk'...
remote: Counting objects: 9, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 9 (delta 2), reused 8 (delta 1), pack-reused 0
Unpacking objects: 100% (9/9), done.
Checking connectivity... done.
geretz$ cd merge-quirk
geretz$ git checkout merge-dst
Branch merge-dst set up to track remote branch merge-dst from origin.
Switched to a new branch 'merge-dst'
geretz$ git merge origin/merge-src
Auto-merging quirk.c
CONFLICT (content): Merge conflict in quirk.c
Automatic merge failed; fix conflicts and then commit the result.
geretz$ cat quirk.c
cat quirk.c

<<<<<<< HEAD
	// a few comment lines - change 1
	// that existed - change 2
	// in the common ancestor - change 3
	// that get changed - change 4
	if(1)
	{
		if (f(1, 2))
		{
			if (thisLineMightDisapper(42))
=======
	// a few comment lines
	// that existed 
	// in the common ancestor
	// added this line in merge-src branch
	// that get changed
	if(1) {
			if (f(1, 2))
>>>>>>> origin/merge-src
			{
				t = time(0);
			}
		}
	}

	if (anotherFunction(1,2))
	{
		t = time(0)
		f(0);
	}
}
geretz$
```
