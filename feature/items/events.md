# Item Events

Events can be assigned to an item based on NBT.
To add events to an item you need to specify the event once, and add NBT to the item:

## Adding the event to the Global Registry

The following steps should only be run once in the code, else events will be triggered more than once.

### Registry

To begin registering an event, create a registry:

```java
var registry = ItemEvents.getRegistryOrNew(ItemTag.String("item-drop"));
```

Registry creation takes an argument, an `ItemTag`
which defines where to check if the item has this registry.

### Identifier

Once you have your registry, you can create an `identifier`.
Internally, this look at the value of the `ItemTag` specified in the registry, and sees if the value of the `ItemTag` is the same as the `identifier`.

```java
var identifier = registry.identifierOrNew("stone")
```

### Adding Events
After you have your Identifier, simply add the event like any [EventHandler](../events.md)

```java
identifier.addEventCallback(ItemDropEvent.class, dropEvent -> {
    dropEvent.getPlayer().sendMessage("You dropped the stone!");
});
```

## Adding NBT to the item.

Once you have completed the steps above, add the NBT to the item's meta:

```java
ItemStack itemStack = ItemStack.builder(Material.STONE)
        .amount(64)
        .meta(itemMetaBuilder ->
                itemMetaBuilder.canPlaceOn(Set.of(Block.STONE))
                        .canDestroy(Set.of(Block.DIAMOND_ORE))
                        .set(ItemTag.String("item-drop"), "stone")) // NBT initialization here
        .build();
```