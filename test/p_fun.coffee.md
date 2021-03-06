    chai = require 'chai'
    chai.should()
    vm = require 'vm'

    describe 'The module', ->
      it 'should export helpers', ->
        require('..').should.have.property 'helpers'
      it 'should export p_fun', ->
        require('..').should.have.property 'p_fun'

    describe 'p_fun', ->
      it 'should rewrite a function', ->
        res = require('..').p_fun (x) -> x+2
        res.should.be.a 'string'

        fun = eval res
        fun.should.be.a 'function'

        {outcome:fun(4)}.should.have.property 'outcome', 6

      it 'should rewrite a function', ->
        res = require('..').p_fun (x) ->
          sum = 0
          sum += v for v in x
          sum
        fun = eval res
        fun.should.be.a 'function'

        {outcome:fun([1,2,3,4])}.should.have.property 'outcome', 10

      it 'should rewrite a function', ->
        res = require('..').p_fun (x,y) => x in y
        fun = eval res

        {outcome:fun(3,[1,2,3,4])}.should.have.property 'outcome', true

      it 'should add extra properties', ->
        extra = "var data = #{JSON.stringify life:42};"
        res = require('..').p_fun extra, -> data.life
        fun = eval res

        {outcome:fun()}.should.have.property 'outcome', 42

    describe 'p_exec', ->
      it 'should execute code fragments', (done) ->
        res = require('..').p_exec -> alert 'hi'
        sandbox =
          alert: (text) ->
            text.should.equal 'hi'
            done()
        vm.runInNewContext res, sandbox
