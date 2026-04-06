// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BToken {

    string public name = "BCoin";
    string public symbol = "NTC";
    uint8 public decimals = 18;
    uint public totalSupply;

    address public owner;

    mapping(address => uint) public balanceOf;

    event Transfer(address indexed from, address indexed to, uint value);
    event Mint(address indexed to, uint amount);
    event Burn(address indexed from, uint amount);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    constructor(uint initialSupply) {
        owner = msg.sender;
        totalSupply = initialSupply * 10 ** decimals;
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address _to, uint _value) public returns (bool) {
        require(balanceOf[msg.sender] >= _value, "Low balance");

        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;

        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function mint(address _to, uint _amount) public onlyOwner {
        uint amount = _amount * 10 ** decimals;

        totalSupply += amount;
        balanceOf[_to] += amount;

        emit Mint(_to, amount);
    }

    function burn(uint _amount) public {
        uint amount = _amount * 10 ** decimals;

        require(balanceOf[msg.sender] >= amount, "Low balance");

        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;

        emit Burn(msg.sender, amount);
    }
}
