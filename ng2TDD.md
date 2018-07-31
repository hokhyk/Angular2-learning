# builder design pattern:

# jasmine:
  desribe("Title for a test suite",function(){
    var a,b;
    it("title for a spec", function() {
        var a = true;
        expect(a).toBe(true);
        expect(b).not.toBe(ture);
    });
  });
  
matchers:  toBe, toEqual, toMatch, toeNull, toBeDefined, toBeUndefined, toContain.

# jasmine's global setup and teardown functions
beforeEach, afterEach, beforeAll, afterAll

# spies
toHaveBeenCalled, toHaveBeenCalledTimes

returnValue, returenValues, callThrough, call, stub, call.allArgs, call.first, call.reset

describe("A sample test suite to test jasmine assertion", function(){
      var myObj, a, fetchA;
      beforeEach( function() {
        myObj = {
          setA: function(value){
            a = value;
          },
          getA: function(){
            return a;
          },
        };
        spyOn(myObj, "getA").and.returnValue(789);
        myObj.setA(123);
        fetchA = myObj.getA();
      });
      
      it("tracks that the spy was called", function(){
        expect(myObj.getA).toHaveBeenCalled();
      });
      
      it("should not affect other functions", function(){
        expect(a).toEqual(123);
      });
      
      it("when called returns the requested value", function(){
        expect(fetchA).toEqual(789);
      });
  });

  
