# Classes-and-Interfaces-With-Unity-and-C-Sharp

When to Use?

When unrelated classes require similar functionality, but different implementations.
See example of Sword/Shield below.
If implementations are the same/shared, use a regular or abstract class.
When you are already inheriting from a class, but need more behaviour.

How they work
Interfaces have no implementation. They are a contract/template that an implementing class must match.

public interface IDamageable
{
	void ApplyDamage(float amount);
}

This interface says that any class that uses it must have an ApplyDamage function that takes a variable of type float.

public class TestClass : MonoBehaviour, IDamageable
{
	public float _currentHealth = 100;

	// Implementation of IDamageable
	public void ApplyDamage(float amount)
	{
		_currentHealth -= amount;
	}
}

TestClass implements the IDamageable interface. If you do not implement every function/property specified in the interface, you will get a compile error.

Examples
1. Sword, Dagger, ThrowingDagger and Shield
Inheritance can break down when your hierachy becomes too deep. In Unity, there’s rarely a reason for inheritance to go beyond a few levels deep.

Imagine you have Sword, Dagger, ThrowingDagger and Shield Classes.

You decide to make Sword/Dagger/ThrowingDagger Inherit from Weapon so they can use its Attack() function. You also make Shield Inherit from Armor.

ThrowingDagger is a ranged weapon, which attacks in a different way to the Attack() in the Weapon Class, so you introduce Melee and Ranged Classes which override the Weapon’s Attack().

Problem
Later, you decide that Shields can also attack (shield bash), and can also be thrown.

Now you’re stuck, because you can only Inherit from one Class. Do you make Shield inherit from Melee or Ranged? Then it would no longer be Armor…

Solution
One solution is to use Interfaces. It will allow unrelated Classes to have the same behaviours.

Note these Interfaces:

IWeapon
IArmor
IThrowable
IDamageDealer
IEquippable
IDancer

Notice that DancingShieldDaggerOfDoom is using all of the Interfaces, so it can behave like a weapon, a shield, a throwable, a damagedealer and a dancer, yet is not related to any other Class. Although not related to anything, it could still be added to a List of IEquippables, for example, alongside unrelated Classes.

Notice that Shield can now Attack() and Throw(), etc.

Notice that Sword/Dagger/ThrowingDagger still inherit from Weapon. It’s Weapon that uses the Interfaces. Only ThrowingDagger implements the IThrowable Interface.

Here’s what some of the Classes might look like:

public class Weapon : IWeapon, IDamageDealer, IEquippable {

	// IWeapon
	public virtual void Attack()
	{
		// Default weapon attack goes here
		// Virtual so it can be overridden by subclasses
	}

	// IDamageDealer
	public virtual void DealDamage(IDamageable damageable, int amount)
	{
		
	}

	// IEquippable
	public virtual void Equip()
	{
		
	}

	public virtual void Unequip()
	{
		
	}

}


public class ThrowingDagger : Weapon, IThrowable {

	// Overrides the default Attack() from the Weapon class
	public override void Attack()
	{
		Throw();
	}

	// IThrowable
	public void Throw()
	{
		
	}

}


public interface IWeapon
{
	void Attack();
}

public interface IDamageDealer
{
	void DealDamage(IDamageable damageable, int amount);
}

public interface IDamageable
{
	void TakeDamage(int amount);
}

public interface IEquippable
{
	void Equip();
	void Unequip();
}


2. Damageable objects

Interfaces can be used to “tag” classes as Damageable. This is an alternative to using a Damageable Component.

public interface IDamageable
{
	void ApplyDamage(float amount);
}
You can check GameObjects for an interface using GetComponent like so:

IDamageable damageable = GetComponent<IDamageable>();

if (damageable != null)
{
	// This GameObject has a script that implements IDamageable
	damageable.ApplyDamage(100);
}
