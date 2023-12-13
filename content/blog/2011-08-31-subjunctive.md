title = "The Subjunctive is not DRY or the Simplest Thing… in BDD"
date = "2011-08-31T11:44:00Z"
template = "main"
tags = []
---
The typical pattern for writing BDD is as follows ([RSpec](http://relishapp.com/rspec/docs/two-minute-tutorial) example):

    describe Animal
      it "should eat"
      it "should have an angry noise"
      it "should have a happy noise"

      context "when first born" do 
        it "should be hungry"
      end

      context "having just eaten" do
        it "should not be hungry"
      end               
    end

While it is natural to use the subjunctive when describing a future possibility, these statements will live on long after the software specified has gone from being a nice idea to living code. Wouldn't it be simpler to use the indicative? 

    describe Animal
      it "eats food"
      it "has an angry noise"
      it "has a happy noise"

      context "when first born" do 
        it "is hungry"
      end

      context "having just eaten" do
        it "is hungry"
      end
    end

Not only does this sound more lively and saves an extra word in each statement, it also makes more sense in the output of the test runner. While a statement beginning with _should_ is always true as long as the requirement remains valid, a statement in the present tense is either true or false depending on whether the code actually does what the requirement says it should do--which is exactly what the test runner is telling us.
