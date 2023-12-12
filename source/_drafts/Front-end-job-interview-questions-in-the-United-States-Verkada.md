---
title: Front-end job interview questions in US
date: 2023-12-06 15:46:55
tags:
categories: Front-end interview
---
# Verkada
## First round
time: 1 hour
1. self introduction
2. achieve debounce
3. talk one of my project

# Altruist
## First round
time: 1h
1. self introduction
2. render table
Welcome to the Altruist Coding Interview Challenge!

Today we will be working with the following files:

The GraphQL Server -> 
pages/api/graphql.ts
The Apollo Client Query Definitions -> 
graphql/queries.ts
and the frontend page -> 
app/page.tsx
Please stay tuned for instructions from your Interviewer

page.tsx
```
"use client";

import { useQuery } from "@apollo/client";
import { TRANSACTION_QUERY } from "@/graphql/queries";
import {
  Table,
  TableHeader,
  TableRow,
  TableCell,
  TableBody,
} from "@/components/ui/table";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";

export default function Home() {
  const { data, loading, error } = useQuery(TRANSACTION_QUERY);
  console.log("data:", data);
  // type, amount, user full name
  // if (loading) return
  if (!data || !data.transactions?.length) return null;
  return (
    <div>
      <h1 className="mb-4 text-4xl font-extrabold leading-none tracking-tight text-gray-900 md:text-5xl lg:text-6xl">
        Altruist
      </h1>
      <div className="flex w-full max-w-sm items-center space-x-2">Todo..</div>
      <div>
        <Table>
          <TableHeader>
            <TableRow>
              <TableCell>Type</TableCell>
              <TableCell>Amount</TableCell>
              <TableCell>user full name</TableCell>
            </TableRow>
          </TableHeader>
          <TableBody>
            {/* <TableRow> */}
            {data.transactions.map((item) => (
              <TableRow>
                <TableCell key={item.id}>{item.type}</TableCell>
                <TableCell key={item.id}>{item.amount}</TableCell>
                <TableCell key={item.id}>{item.fullName}</TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </div>
    </div>
  );
}

```
pages/api/graphql.ts
```
import { ApolloServer } from "@apollo/server";
import { startServerAndCreateNextHandler } from "@as-integrations/next";

/**
 * This is my GraphqQL SERVER
 * In here I should use these 3 URLs
 *
 * Users https://618b624fded7fb0017bb8f25.mockapi.io/users
 * User https://618b624fded7fb0017bb8f25.mockapi.io/users/:id
 * Transactions https://618b624fded7fb0017bb8f25.mockapi.io/transactions
 */

const typeDefs = `#graphql
  type Query {
    users: [User]
    transactions: [Transaction]
  }
  type User {
    id: ID
    firstName: String
    lastName: String
  }
  type Transaction {
    id: ID
    # userId: String
    fullName: String
    amount: String
    type: String
    # firstName: String
    # lastName: String
  }
`;

const resolvers = {
  Query: {
    users: async () => {
      const usersResponse = await fetch(
        "https://618b624fded7fb0017bb8f25.mockapi.io/users",
      );

      return await usersResponse.json();
    },
    transactions: async () => {
      // const usersResponse = await fetch(
      //   "https://618b624fded7fb0017bb8f25.mockapi.io/users",
      // )
      //   .then((res) => {
      //     if (res.ok) {
      //       return res.json();
      //     } else {
      //       return [];
      //     }
      //     console.log("res:", res.ok);
      //   })
      //   .catch((e) => {
      //     console.error("user error:", e);
      //     return [];
      //   });

      // const userData = await usersResponse.json();
      const userData = userMockData;
      const userInfo = {};
      userData.forEach((user) => {
        userInfo[user.id] = user;
      });
      const transactionData = transactionMockData;
      // const transactionsResponse = await fetch(
      //   "https://618b624fded7fb0017bb8f25.mockapi.io/transactions",
      // );

      // const transactionData = await transactionsResponse.json();
      transactionData.forEach((transaction) => {
        const curUser = userInfo[transaction.userId];
        if (curUser) {
          transaction["fullName"] = `${curUser.firstName} ${curUser.lastName}`;
        }
      });
      return transactionData;
      // return [];
    },
  },
};

const server = new ApolloServer({ resolvers, typeDefs });

export default startServerAndCreateNextHandler(server);

```
graphql/queries.ts
```
import { gql } from "@apollo/client";

/**
 * These are the queries my React Components will make
 */

export const USER_QUERY = gql`
  query Users {
    users {
      firstName
    }
  }
`;

export const TRANSACTION_QUERY = gql`
  query Transactons {
    transactions {
      id
      # userId
      amount
      type
      fullName
    }
  }
`;

```