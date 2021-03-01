## Underlying philosophy

Why does one project within an organization flourish while another project with the same fundamental architectural needs, within the same organization, flounders? Often, the root of the problem is a lack of overarching standard that keeps development on a steady and smooth line. What we aim to achieve here as a team; is a progressive and collective effort of creating a grade-A software product; the one that we are not afraid to modify and extend; the one that we are not horrified to refactor. This, my child, is the art of software engineering. This document aims to shed lights on fundamental standards that we have defined here at Ezyvet. Before getting into actual coding decision and dos and don’ts, we first need to review our underlying philosophy towards development. Here are 17 philosophies that we stick to at any cost (heavily inspired by the Zen of Python):

Explicit is better than implicit.

Simple is better than complex.

Complex is better than complicated.

Flat is better than nested.

Indeed, the ratio of time spent reading versus writing is well over 10 to 1. We are constantly reading old code as part of the effort to write new code. ...[Therefore,] making it easy to read makes it easier to write.

It is not enough for code to work

A long descriptive name is better than a short enigmatic name. A long descriptive name is better than a long descriptive comment.

You should name a variable using the same care with which you name a first-born child.

Clean code always looks like it was written by someone who cares.

The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that.

Special cases aren't special enough to break the rules.

Errors should never pass silently. Unless explicitly silenced.

In the face of ambiguity, refuse the temptation to guess.

There should be one-- and preferably only one --obvious way to do it.

Now is better than never.

If the implementation is hard to explain, it's a bad idea.

If the implementation is easy to explain, it may be a good idea.

Introduction

This document aims to increase constancy to best practices and leads devs away from anti-patterns. For that purpose please read this document carefully and reach out to the frontend tech lead whenever necessary.

1. Commenting Code

Readability is one of the most important factors for any enterprise application. Why?

Reason #1

In an organization, we work in a team; different people work on different parts of the project. It’s hard to fathom the whole system on top of your head, thus help the fellow dev with clear and succinct comments.

Reason #2

This will help you understand what you’ve done as well. Especially if it involves some complex logic. Reason #3

Philosophy number 5Best way to do this ? Use JSDoc. Here’s an example:

/\*\*

- return full name of the user
- @param {string} firstName First Name of the User
- @param {string} lastName Last Name of the User
- @return {string} Fullname of the user
  \*/

function getFullName(firstName, lastName) {
return `${firstName} ${lastName}`
}

If you had complicated operations, always use @example, write code like you care for the next person.

Read more here.

2. Module Importing

Imports should be categorised for increased readability. Below is an example of module importing categories:

Multivariate Dependencies

If you had to import various dependencies such as types, utils and hooks from the same library, categorise it under multivariate dependencies. This should be at top. Be aware that we don’t use the phrase constants, and if you’ve got any constants, you will just categorise it under types. We favour the phrase utils over helpers.The categories are as followings;

Multivariate Dependencies

Components

Styles

Utils

Types

Hooks

here is an example:

// Multivariate Dependencies
import React, { useState, ReactElement } from 'react';

// Components
import { Grid, Table } from '@material-ui/core';
import { someComponent } from 'path/componentPath';

// Utils
import { connect } from 'redux';
import { someUtils } from 'path/utilsPath';

// Types
import { formikProps } from 'formik';
import { someType } from 'path/typePath';

3. Code organization

Code organization is a critical matter. It it essential to modularise pieces of functioanlity and abstract out only what is necessary. Encapsulation and abstraction are very basic matters, but often neglected. We do follow this folder structure for components:

├── components
│ ├── styles
│ ├── utils
│ ├── types
│ ├── tests
│ ├── components
│ ├── index.tsx

ofc you only include component specific styles, tests, utils, and components inside this folder. The index file is the gateway that abstracts out this module. The overall folder structure looks as below:

├── components
│ ├── styles
│ ├── utils
│ ├── types
│ ├── tests
│ ├── components
│ ├── index.tsx
├── graphql
│ ├── queries
│ ├── fragments
│ ├── index.tsx
├── hooks
│ ├── /// hooks categories
│ ├── index.tsx
├── layouts
│ ├── /// layout categories
│ ├── index.tsx
├── pages(views)
│ ├── /// pages categories
│ ├── index.tsx
├── static
│ ├── svgs
│ ├── images
│ ├── icons
│ ├── fonts
│ ├── index.tsx
├── styles (common styles)
│ ├── /// styles categories
│ ├── index.tsx
├── types (common types)
│ ├── /// types categories
│ ├── index.tsx
├── utils (common utils)
│ ├── /// utils categories
│ ├── index.tsx
├── redux
│ ├── actions
│ ├── reducers
│ ├── saga
│ ├── selectors
│ ├── index.tsx

4. Keep component clean! like really clean

Any fool can write code that a computer can understand. Good programmers write code that humans can understand.

No component should be more than 150 lines! This is very important for the Ezyvet frontend team. You should never have your types in your component, you should never declare styles in the component, you should never have another component defined in the same file. One file just exports one component and that’s it. Here is an example of a clean component:

// Multivariate Dependencies
import React from 'react';

// Components
import { SortableContainer } from '../components/SortableContainer';
import { MetaRecordsDescription } from '../components/MetaRecordsDescription';

// Utils
import { configAggregator } from '../utils/helpers/configAggregator';

// Types
import { MetaRecordsFormProps } from '../types/MetaRecordsFormTypes';
import { ExtendedInvoiceLine } from '../../records/redux/selectors/invoice/getTableConfig';

export const MetaRecordsForm = ({
defaultColumnConfig,
columnMetaConfig,
}: MetaRecordsFormProps<ExtendedInvoiceLine>) => {
return (
<>
<MetaRecordsDescription />
<SortableContainer
aggregatedConfig={configAggregator(
defaultColumnConfig,
columnMetaConfig
)} ></SortableContainer>
</>
);
};

5. Module exporting

Here in Ezyvet we only do named exports. Please avoid default exports at any costs. A good example is this:
