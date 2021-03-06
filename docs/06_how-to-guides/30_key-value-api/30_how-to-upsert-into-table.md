---
content_title: How-To Upsert Into Key-Value Table
link_text: "How-To Upsert Into Key-Value Table"
---

## Summary

This how-to procedure provides instructions to upsert into `Key-Value Table` (`kv table`).

Use the method `put` defined by the `eosio::kv::table` type to accomplish this task.

## Prerequisites

Before you begin, complete the following prerequisites:

* An EOSIO development environment, for details consult the [Get Started](https://developers.eos.io/welcome/latest/getting-started/development-environment/introduction) Guide
* A smart contract, let’s call it `smrtcontract`
* A user defined type, let’s call it `person`, which defines the data which is stored in the table
* A `kv table` type which stores objects of type `person`, let’s call it `address_table`. The primary index of the `kv table` is defined based on the `person::account_name` property.

Refer to the following possible implementation of your starting point.

`smartcontract.hpp file`

```cpp
struct person {
  eosio::name account_name;
  std::string first_name;
  std::string last_name;
  std::string personal_id;
};

class [[eosio::contract]] smrtcontract : public contract {
    struct [[eosio::table]] address_table : eosio::kv::table<person, "kvaddrbook"_n> {

     index<name> account_name_uidx {
        name{"accname"_n},
        &person::account_name };

     address_table(name contract_name) {
        init(contract_name, account_name_uidx)
     }
  };
  public:
     using contract::contract;
};
```

## Procedure

Complete the following steps to insert a new `person` object, and then update it, in the `kv table`:

1. Create a new action in your contact, let’s call it `upsert`, which takes as input parameters an account name, a first name, a last name and a personal id.
2. In the `upsert` action access the instance of `address_table` by declaring a local variable of `address_table` type.
3. And then call the `put` method of the `address_table` and pass to it a newly created `person` object based on the action’s input parameters.

Refer to the following possible implementation to insert a new `person` object, and then update it, in the `kv table`:

`smartcontract.hpp file`

```cpp
class [[eosio::contract]] smrtcontract : public contract {
    struct [[eosio::table]] address_table : eosio::kv::table<person, "kvaddrbook"_n> {

     index<name> account_name_uidx {
        name{"accname"_n},
        &person::account_name };

     address_table(name contract_name) {
        init(contract_name, account_name_uidx)
     }
  };
  public:
     using contract::contract;

     // creates if not exists, or updates if already exists, a person
     [[eosio::action]]
     void upsert(eosio::name account_name,
        string first_name,
        string last_name,
        string country,
        string personal_id);

     using upsert_action = action_wrapper<"upsert"_n, &smrtcontract::upsert>;
};
```

`smartcontract.cpp file`

```cpp
// creates if not exists, or updates if already exists, a person
[[eosio::action]]
void smrtcontract::upsert(
     eosio::name account_name,
     string first_name,
     string last_name,
     string personal_id) {
  address_table addresses{"kvaddrbook"_n};

  // call put which upserts into kv table
  addresses.put({account_name, first_name, last_name, personal_id}, get_self());
}
```

## Next Steps

The following options are available when you complete the procedure:

* Verify if the newly inserted `person` actually exists in the table. To accomplish this task, use the `exists()` function of any index defined for the table.
* Retrieve the newly inserted or updated `person` from the table. To accomplish this task, use the `find()` function of any index defined for the table.
* Delete the newly created or updated `person` from the table. To accomplish this task, use the `erase()` function of the `kv table`.
