# Eth-CrypTulpar-Game
A game which based ethereum
&&&&&&&&&&&&& a basic smart contrat &&&&&&&&&&&&&&&&

pragma solidity ^0.4.0;
contract Crt {
mapping(address =>uint256) public amount;
uint256 totalAmount;
string tokenName;
string tokenSymbol;
uint256 decimal;
constructor() public{
totalAmount = 10000 * 10**10;
amount[msg.sender]= totalAmount;
tokenName = "CrypTulpar";
tokenSymbol= "Crt";
decimal= 10;
}
function totalSupply() public view returns(uint256){
return totalAmount;
}
function balanceOf(address to_who) public view
returns(uint256){
return amount[to_who];
}
function transfer(address to_a,uint256 _value) public
returns(bool){
require(_value<=amount[msg.sender] , "Even value required") ;
amount[msg.sender]=amount[msg.sender]-_value;
amount[to_a]=amount[to_a]+_value;
return true;
}
}




&&&&&&&&&&&&&cerate a new folder that Contract &&&&&


.............create a new folder that Characters in Contract &&&&&

.............create a new file that NonPlayAble in Character folder...................
pragma solidity ^0.4.0;

import "../Character.sol";

contract NonPlayableCharacter is Character {
}

............create a new file taht PlayableCharacter in Character folder ..............
pragma solidity ^0.4.0;

import "../Character.sol";

contract PlayableCharacter is Character {
}

.............cerate a new folder is that classes in Contract folder &&&&&&&&&

pragma solidity ^0.4.0;

import "../Class.sol";
import "../Skills/Attack.sol";
import "../Skills/Backstab.sol";

contract Rogue is Class("Rogue", 3, 150, 15, 10, 5, 5) {
    constructor() public {
        _skills.push(new Attack());
        _skills.push(new Backstab());
    }
}

import "../Class.sol";
import "../Skills/Attack.sol";

contract Rogue is Class("Troll", 11, 50, 0, 2, 0, 2) {
    constructor() public {
        _skills.push(new Attack());
    }
}

import "../Class.sol";
import "../Skills/Attack.sol";
import "../Skills/Cleave.sol";

contract Warrior is Class("Warrior", 1, 200, 10, 2, 2, 16) {
    constructor() public {
        _skills.push(new Attack());
        _skills.push(new Cleave());
    }
}

import "../Class.sol";
import "../Skills/Attack.sol";
import "../Skills/Fireball.sol";

contract Wizard is Class("Wizard", 2, 100, 20, 2, 16, 2) {
    constructor() public {
        _skills.push(new Attack());
        _skills.push(new Fireball());
    }
}

..........create a new folder is that Libraies in Contract folder&&&&&&&&

pragma solidity ^0.4.0;

/**
 * @title SafeMath
 * @dev Math operations with safety checks that revert on error
 */
library SafeMath {

    /**
    * @dev Multiplies two numbers, reverts on overflow.
    */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-solidity/pull/522
        if (a == 0) {
        return 0;
        }

        uint256 c = a * b;
        require(c / a == b);

        return c;
    }

    /**
    * @dev Integer division of two numbers truncating the quotient, reverts on division by zero.
    */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0); // Solidity only automatically asserts when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
    * @dev Subtracts two numbers, reverts on overflow (i.e. if subtrahend is greater than minuend).
    */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);
        uint256 c = a - b;

        return c;
    }

    /**
    * @dev Adds two numbers, reverts on overflow.
    */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a);

        return c;
    }

    /**
    * @dev Divides two numbers and returns the remainder (unsigned integer modulo),
    * reverts when dividing by zero.
    */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0);
        return a % b;
    }
}

......... create a new folder is that Skills in Contract

pragma solidity ^0.4.0;

import "../Skill.sol";

contract Attack is Skill("Attack", 0, 1, 1, 0, 1) {}

import "../Skill.sol";

contract Backstab is Skill("Backstab", 3, 1, 10, 2, 1) {}

import "../Skill.sol";

contract Cleave is Skill("Cleave", 1, 1, 6, 2, 1) {}

import "../Skill.sol";

contract Fireball is Skill("Fireball", 2, 1, 15, 2, 2) {}

.......... create a new file Character.sol in Contract

pragma solidity ^0.4.0;

import "./Class.sol";
import "./Libraries/SafeMath.sol";

contract Character {
    
    using SafeMath for uint;

    address private _game;

    string private _name;
    Class private _class;

    uint private _experience;
    uint private _level;

    uint private _hitPoints;
    uint private _manaPoints;

    uint private _agility;
    uint private _intelligence;
    uint private _strength;

    uint8 private _set;

    modifier isGame {
        require(msg.sender == _game, "Request did not come from the game");
        _;
    }

    constructor(string memory name, Class class) public {
        _game = msg.sender;
        _name = name;
        _class = class;
        _set = 1;
    }

    function getName() public view returns(string memory) {
        return _name;
    }

    function getClass() public view returns(Class) {
        return _class;
    }

    function getHitPoints() public view returns(uint) {
        return _hitPoints.add(_class.getHitPoints());
    }

    function getManaPoints() public view returns(uint) {
        return _manaPoints.add(_class.getManaPoints());
    }

    function getAgility() public view returns(uint) {
        return _agility.add(_class.getAgility());
    }

    function getIntelligence() public view returns(uint) {
        return _intelligence.add(_class.getIntelligence());
    }

    function getStrength() public view returns(uint) {
        return _strength.add(_class.getStrength());
    }

    function getExperience() public view returns(uint) {
        return _experience;
    }

    function getLevel() public view returns(uint) {
        return _level;
    }

    function isSet() public view returns(uint8) {
        return _set;
    }

    function setLevel(uint level) public isGame {
        _level = level;
    }

    function setExperience(uint experience) public isGame {
        _experience = experience;
    }

    function setHitPoints(uint hitPoints) public isGame {
        _hitPoints = hitPoints;
    }

    function setManaPoints(uint manaPoints) public isGame {
        _manaPoints = manaPoints;
    }

    function setAgility(uint agility) public isGame {
        _agility = agility;
    }

    function setIntelligence(uint intelligence) public isGame {
        _intelligence = intelligence;
    }

    function setStrength(uint strength) public isGame {
        _strength = strength;
    }

}

..............create a new file Class.sol..............

pragma solidity ^0.4.0;

import "./Skill.sol";

contract Class {
    string private _name;

    uint private _class;
    
    uint private _hitPoints;
    uint private _manaPoints;

    uint private _agility;
    uint private _intelligence;
    uint private _strength;

    Skill[] internal _skills;

    constructor(
        string memory name,
        uint class,
        uint hitPoints,
        uint manaPoints,
        uint agility,
        uint intelligence,
        uint strength
    ) public {
        _name = name;
        _class = class;
        _hitPoints = hitPoints;
        _manaPoints = manaPoints;
        _agility = agility;
        _intelligence = intelligence;
        _strength = strength;
    }

    function getName() public view returns(string memory) {
        return _name;
    }

    function getClass() public view returns(uint) {
        return _class;
    }

    function getHitPoints() public view returns(uint) {
        return _hitPoints;
    }

    function getManaPoints() public view returns(uint) {
        return _manaPoints;
    }

    function getAgility() public view returns(uint) {
        return _agility;
    }

    function getIntelligence() public view returns(uint) {
        return _intelligence;
    }

    function getStrength() public view returns(uint) {
        return _strength;
    }
}

........a new file Combat Engine ............

pragma solidity ^0.4.0;

import "./Game.sol";
import "./Character.sol";
import "./Skill.sol";
import "./Libraries/SafeMath.sol";

contract CombatEngine {
    
    uint private PYHSICAL_SKILL = 1;
    uint private MAGICAL_SKILL = 2;
    
    using SafeMath for uint;

    function attack(Character attacker, Character defender, Skill skill) public view returns (uint) {
        if (PYHSICAL_SKILL == skill.getSkillType()) {
            return ((attacker.getStrength() * 2) + attacker.getAgility()) - 
                ((defender.getAgility() * 2) + defender.getStrength()) + 
                (attacker.getAgility() / 4);
        } else if (MAGICAL_SKILL == skill.getSkillType()) {
            return ((attacker.getIntelligence() * 2) + attacker.getAgility() + skill.getDamage()) - 
                ((defender.getAgility() * 2) + defender.getIntelligence());
        }
        return 0;
    }
}


............a new file is Game.sol ...........

pragma solidity ^0.4.0;

import "./Character.sol";
import "./Class.sol";
import "./CombatEngine.sol";
import "./Skill.sol";

contract Game {
    
    address private _owner;

    CombatEngine private _combatEngine;

    mapping(address => Character) private _characters;
    mapping(string => Class) private _classes;

    modifier isOwner {
        require(_owner == msg.sender, "Sender is not the contract owner");
        _;
    }

    modifier hasCharacter {
        require(_characters[msg.sender].isSet() == 1, "Character does not exist for address");
        _;
    }

    constructor() public {
        _owner = msg.sender;
        _combatEngine = new CombatEngine();
    }

    function createCharacter(string memory name, string memory class) public {
        _characters[msg.sender] = new Character(name, _classes[class]);
    }

    function getCharacter() public view returns (Character) {
        return _characters[msg.sender];
    }

    function addClass(string memory key, Class class) public isOwner {
        _classes[key] = class;
    }

    function removeClass(string memory key) public isOwner {
        delete _classes[key];
    }

    function attack(Character character, Skill skill) public view returns(uint) {
        return _combatEngine.attack(_characters[msg.sender], character, skill);
    }
}


..........a new file Skill.sol...........


pragma solidity ^0.4.0;

contract Skill {
    string private _name;
    uint private _class;
    uint private _levelRequired;
    uint private _damage;
    uint private _manaCost;
    uint private _skillType;

    constructor(string memory name, uint class, uint levelRequired, uint damage, uint manaCost, uint skillType) public {
        _name = name;
        _class = class;
        _levelRequired = levelRequired;
        _damage = damage;
        _manaCost = manaCost;
        _skillType = skillType;
    }

    function getName() public view returns(string memory) {
        return _name;
    }

    function getClass() public view returns (uint) {
        return _class;
    }

    function getLevelRequired() public view returns(uint) {
        return _levelRequired;
    }

    function getDamage() public view returns (uint) {
        return _damage;
    }

    function getManaCost() public view returns (uint) {
        return _manaCost;
    }

    function getSkillType() public view returns (uint) {
        return _skillType;
    }
}
