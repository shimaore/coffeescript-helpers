    chai = require 'chai'
    chai.should()

    describe 'The module', ->
      it 'should export helpers', ->
        require('..').should.have.property 'helpers'
      it 'should export p_fun', ->
        require('..').should.have.property 'p_fun'

      it 'should rewrite a function', ->
        res = require('..').p_fun (x) -> x+2
        res.should.be.a 'string'

        fun = eval res
        fun.should.be.a 'function'

        {outcome:fun(4)}.should.have.property 'outcome', 6
