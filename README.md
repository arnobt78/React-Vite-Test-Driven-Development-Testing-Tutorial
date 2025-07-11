# Focus Flow - React-Vite Test-Driven Development (TDD) Testing Tutorial (Vitest)

<img width="617" height="475" alt="Screenshot 2025-06-18 at 13 58 33" src="https://github.com/user-attachments/assets/0c3a88d2-7985-41ab-8e44-e5fae93ef1f6" /> <img width="882" height="276" alt="Screenshot 2025-06-18 at 13 58 10" src="https://github.com/user-attachments/assets/f0c9fa32-55d8-4953-94d0-2c5c5c8d59f5" />

---

A productivity app built with React, TypeScript, and Tailwind CSS, designed to help you manage tasks by priority. This project is developed using a **Test-Driven Development (TDD)** approach, making it an excellent resource for learning both React and modern testing practices.

---

## Table of Contents

1. [Project Summary](#project-summary)
2. [Features](#features)
3. [Technology Stack](#technology-stack)
4. [Project Structure](#project-structure)
5. [Getting Started](#getting-started)
6. [App Walkthrough](#app-walkthrough)
7. [TDD & Testing Guide](#tdd--testing-guide)
8. [Teaching & TDD Walkthrough](#teaching--tdd-walkthrough)
9. [Keywords](#keywords)
10. [License](#license)

---

## Project Summary

**Focus Flow** is a simple, extensible task management app that demonstrates how to build robust React applications using TDD. The project guides you through writing tests first, then implementing features, ensuring your code is reliable and maintainable.

---

## Features

- Add, view, and delete tasks with categories (urgent, important, normal, low)
- Responsive UI with Tailwind CSS
- Uses React Context for state management
- Fully tested with Vitest and React Testing Library
- Clean, modular codebase for easy learning and extension

---

## Technology Stack

- **React** (with Hooks)
- **TypeScript**
- **Tailwind CSS**
- **Vite** (for fast development)
- **Vitest** (unit/integration testing)
- **React Testing Library** (component testing)
- **lucide-react** (icon library)

---

## Project Structure

```bash
├── public/
├── src/
│   ├── __tests__/
│   │   ├── AppWithContext.test.tsx
│   │   ├── Form.test.tsx
│   │   ├── ItemCard.test.tsx
│   │   └── List.test.tsx
│   ├── assets/
│   ├── components/
│   │   ├── App.tsx
│   │   ├── Form.tsx
│   │   ├── ItemCard.tsx
│   │   └── List.tsx
│   ├── AppWithContext.tsx
│   ├── FlowContext.tsx
│   ├── index.css
│   ├── main.tsx
│   ├── utils.ts
│   └── vite-env.d.ts
├── package.json
├── tailwind.config.js
├── vite.config.ts
└── README.md
```

---

## Getting Started

1. **Install dependencies:**

   ```sh
   npm install
   ```

2. **Run the development server:**

   ```sh
   npm run dev
   ```

3. **Run tests:**

   ```sh
   npm test
   ```

   or

   ```sh
   npx vitest
   ```

---

## App Walkthrough

- **Main Logic:**  
  The core logic (types and hooks) is in `src/utils.ts`. The `useFlowManager` hook manages the list of tasks and provides add/delete functionality.

- **Components:**

  - `Form.tsx`: Handles task creation.
  - `List.tsx`: Displays all tasks.
  - `ItemCard.tsx`: Shows individual task details and delete button.

- **Context:**  
  `FlowContext.tsx` provides global state using React Context, making the app scalable and testable.

- **App Entry:**  
  `AppWithContext.tsx` is the main app, wrapped in `FlowProvider` in `main.tsx`.

---

## TDD & Testing Guide

This project is built using **Test-Driven Development**. All major components have corresponding tests in `src/__tests__/`.

### Form Component Tests

- Test initial render (fields are empty)
- Test form submission (calls `onSubmit` with correct data)
- Test validation (required fields)
- Test form reset after submission

### List Component Tests

- Renders heading and correct number of `ItemCard`s
- Handles empty state

### ItemCard Component Tests

- Renders task details
- Calls `onDelete` when delete button is clicked

### Context & Integration Tests

- Full app integration tests in `AppWithContext.test.tsx`
- Add and delete tasks through the UI
- Ensure context state updates and UI reflects changes

**Testing Tools Used:**

- [Vitest](https://vitest.dev/)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)

---

## Teaching & TDD Walkthrough

Below is a detailed, step-by-step teaching walkthrough of the TDD process for this project, including code snippets, test strategies, and implementation notes. Use this as a learning resource for React, TDD, and component testing.

## Main Logic

To make things simpler, we’ll start by building the core logic of the application first. Once that’s complete, we’ll move on to writing the tests. This approach will allow you to see the bigger picture of how the application works before diving into the details of test setup.

- create `src/utils.ts` file

This file contains all of the types and the main logic of the application.

```tsx
import { useState } from "react";

export type ItemCategory = "urgent" | "important" | "normal" | "low";

export type Item = {
  id: string;
  title: string;
  description: string;
  category: ItemCategory;
};

export type ItemWithoutId = Omit<Item, "id">;

export const useFlowManager = () => {
  const [items, setItems] = useState<Item[]>([]);

  const handleAddItem = (newItem: ItemWithoutId) => {
    setItems([...items, { ...newItem, id: Date.now().toString() }]);
  };

  const handleDeleteItem = (id: string) => {
    setItems(items.filter((item) => item.id !== id));
  };

  return { items, handleAddItem, handleDeleteItem };
};
```

## Components

- create `src/components/Form.tsx` file
- create `src/components/List.tsx` file
- create `src/components/ItemCard.tsx` file

```tsx
const Form = () => {
  return <div>Form</div>;
};
export default Form;
```

## App.tsx

```tsx
import Form from "./components/Form";
import List from "./components/List";
import { useFlowManager } from "./utils";

export default function Home() {
  const { items, handleAddItem, handleDeleteItem } = useFlowManager();
}
```

In order to fix we need to setup props for the components.

components/Form.tsx

```tsx
import { type ItemWithoutId, type ItemCategory } from "../utils";

const Form = ({ onSubmit }: { onSubmit: (item: ItemWithoutId) => void }) => {
  return <div>Form</div>;
};
export default Form;
```

```tsx
import ItemCard from "./ItemCard";
import { Item } from "../utils";

const List = ({
  items,
  onDelete,
}: {
  items: Item[];
  onDelete: (id: string) => void;
}) => {
  return <div>List</div>;
};
export default List;
```

## Form Tests

`src/__tests__/Form.test.tsx`

For the Form component I'm going to setup tests one by one but you can set all of them at once.

1. Set up the test environment:

   - Import necessary testing libraries (React Testing Library, Vitest)
   - Import the component to be tested

2. Create helper functions:

   - Define a function to get commonly used elements (e.g., `getElements`)

3. Set up the test suite:

   - Use `describe` to group related tests
   - Create mock functions for callbacks (e.g., `mockOnSubmit`)

4. Write individual test cases:

   - Use `test` or `it` to define each test case
   - Render the component with necessary props
   - Interact with the component using `userEvent`
   - Make assertions using `expect`

5. Test initial render:

   - Check if form fields are empty initially

6. Test form submission:

   - Fill out form fields
   - Submit the form
   - Verify if the submit callback is called with correct data

7. Test form validation:

   - Try submitting an empty form
   - Verify that the submit callback is not called

8. Test form reset after submission:

   - Fill out and submit the form
   - Check if fields are cleared after submission

9. Use `beforeEach` to reset mocks between tests

10. Use async/await for asynchronous operations (e.g., user interactions)

11. Use descriptive test names to clearly indicate what each test is checking

src/**tests**/Form.test.tsx

```tsx
import { render, screen } from "@testing-library/react";
import { describe, test, expect, vi } from "vitest";
import Form from "../components/Form";
import userEvent, { UserEvent } from "@testing-library/user-event";

const getElements = () => ({
  titleInput: screen.getByRole("textbox", { name: /title/i }),
  descriptionInput: screen.getByRole("textbox", { name: /description/i }),
  categorySelect: screen.getByRole("combobox", { name: /category/i }),
  submitButton: screen.getByRole("button", { name: /add task/i }),
});

describe("Form Component", () => {
  let user: UserEvent;
  const mockOnSubmit = vi.fn();

  beforeEach(() => {
    mockOnSubmit.mockClear();
    user = userEvent.setup();
  });

  // 1. Test renders form with empty fields initially
  test("renders form with empty fields initially", () => {
    render(<Form onSubmit={mockOnSubmit} />);

    const { titleInput, descriptionInput, categorySelect } = getElements();

    expect(titleInput).toHaveValue("");
    expect(descriptionInput).toHaveValue("");
    expect(categorySelect).toHaveValue("");
  });
  // 2. Test submits form with entered values
  test("submits form with entered values", async () => {
    render(<Form onSubmit={mockOnSubmit} />);
    const { titleInput, descriptionInput, categorySelect, submitButton } =
      getElements();

    await user.type(titleInput, "New Task");
    await user.type(descriptionInput, "Task Description");
    await user.selectOptions(categorySelect, "urgent");
    await user.click(submitButton);

    expect(mockOnSubmit).toHaveBeenCalledWith({
      title: "New Task",
      description: "Task Description",
      category: "urgent",
    });
  });
  // 3. Test validates required fields
  test("validates required fields", async () => {
    render(<Form onSubmit={mockOnSubmit} />);
    const { submitButton } = getElements();
    await user.click(submitButton);
    expect(mockOnSubmit).not.toHaveBeenCalled();
  });
  // 4. Test clears form after successful submission
  test("clears form after successful submission", async () => {
    render(<Form onSubmit={mockOnSubmit} />);
    const { titleInput, descriptionInput, categorySelect, submitButton } =
      getElements();

    await user.type(titleInput, "New Task");
    await user.type(descriptionInput, "Task Description");
    await user.selectOptions(categorySelect, "urgent");
    await user.click(submitButton);

    expect(titleInput).toHaveValue("");
    expect(descriptionInput).toHaveValue("");
    expect(categorySelect).toHaveValue("");
  });
});
```

src/components/Form.tsx

```tsx
import { useState } from "react";
import { type ItemWithoutId, type ItemCategory } from "../utils";

const Form = ({ onSubmit }: { onSubmit: (item: ItemWithoutId) => void }) => {
  const [title, setTitle] = useState("");
  const [description, setDescription] = useState("");
  const [category, setCategory] = useState<ItemCategory | "">("");

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
  };

  return (
    <div className="max-w-xl">
      <h2 className="text-xl font-semibold mb-2">Add New Task</h2>
      <form onSubmit={handleSubmit} className="space-y-4">
        <div>
          <label
            htmlFor="title"
            className="text-sm font-medium leading-none block mb-2"
          >
            Title
          </label>
          <input
            id="title"
            value={title}
            onChange={(e) => setTitle(e.target.value)}
            required
            className="flex h-10 w-full rounded-md border px-3 py-2 text-sm"
          />
        </div>
        <div>
          <label
            htmlFor="description"
            className="text-sm font-medium leading-none block mb-2"
          >
            Description
          </label>
          <input
            id="description"
            value={description}
            onChange={(e) => setDescription(e.target.value)}
            required
            className="flex h-10 w-full rounded-md border  px-3 py-2 text-sm"
          />
        </div>
        <div>
          <label
            htmlFor="category"
            className="text-sm font-medium leading-none block mb-2"
          >
            Category
          </label>
          <select
            id="category"
            value={category}
            onChange={(e) => setCategory(e.target.value as ItemCategory)}
            required
            className="flex h-10 w-full rounded-md border  px-3 py-2 text-sm"
          >
            <option value="">Select Category</option>
            <option value="urgent">Urgent</option>
            <option value="important">Important</option>
            <option value="normal">Normal</option>
            <option value="low">Low Priority</option>
          </select>
        </div>
        <button
          type="submit"
          className="rounded text-sm font-medium bg-blue-600 text-white hover:bg-blue-700 h-10 px-4 py-2 "
        >
          Add Task
        </button>
      </form>
    </div>
  );
};

export default Form;
```

### Test 2 : Submits form with entered values

```tsx
// 2. Test submits form with entered values
test("submits form with entered values", async () => {
  render(<Form onSubmit={mockOnSubmit} />);
  const { titleInput, descriptionInput, categorySelect, submitButton } =
    getElements();

  await user.type(titleInput, "New Task");
  await user.type(descriptionInput, "Task Description");
  await user.selectOptions(categorySelect, "urgent");
  await user.click(submitButton);

  expect(mockOnSubmit).toHaveBeenCalledWith({
    title: "New Task",
    description: "Task Description",
    category: "urgent",
  });
});
```

src/components/Form.tsx

```tsx
const handleSubmit = (e: React.FormEvent) => {
  e.preventDefault();
  // typescript will complain if we don't check for empty strings
  onSubmit({ title, description, category });
};
```

### Test 3 : Validates required fields

Since we use html input `required` attribute, test will pass right away if we don't fill out all of the inputs in the form.

```tsx
// 3. Test validates required fields
test("validates required fields", async () => {
  render(<Form onSubmit={mockOnSubmit} />);
  const { submitButton } = getElements();
  await user.click(submitButton);
  expect(mockOnSubmit).not.toHaveBeenCalled();
});
```

```tsx
const handleSubmit = (e: React.FormEvent) => {
  e.preventDefault();
  // typescript will complain if we don't check for empty strings
  if (!title || !description || !category) return;
  onSubmit({ title, description, category });
};
```

### Test 4 : Clears form after successful submission

```tsx
// 4. Test clears form after successful submission
test("clears form after successful submission", async () => {
  render(<Form onSubmit={mockOnSubmit} />);
  const { titleInput, descriptionInput, categorySelect, submitButton } =
    getElements();

  await user.type(titleInput, "New Task");
  await user.type(descriptionInput, "Task Description");
  await user.selectOptions(categorySelect, "urgent");
  await user.click(submitButton);

  expect(titleInput).toHaveValue("");
  expect(descriptionInput).toHaveValue("");
  expect(categorySelect).toHaveValue("");
});
```

```tsx
const handleSubmit = (e: React.FormEvent) => {
  e.preventDefault();
  // typescript will complain if we don't check for empty strings
  if (!title || !description || !category) return;
  onSubmit({ title, description, category });
  setTitle("");
  setDescription("");
  setCategory("");
};
```

## List Tests

- create `src/__tests__/List.test.tsx` file

```tsx
import { render, screen } from "@testing-library/react";
import { describe, test, expect, vi } from "vitest";
import List from "../components/List";
import { type Item } from "../utils";
// Mock the ItemCard component to simplify testing
// This replaces the actual ItemCard component with a simple article element
// that contains the text "item card"
vi.mock("../components/ItemCard", () => ({
  // The default export is a function that returns a simple article element
  // This helps us test the List component in isolation without the complexity
  // of the actual ItemCard implementation
  default: () => <article>item card</article>,
}));

describe("List", () => {
  const mockItems: Item[] = [
    {
      id: "1",
      title: "Test Item 1",
      description: "Content 1",
      category: "urgent",
    },
    {
      id: "2",
      title: "Test Item 2",
      description: "Content 2",
      category: "normal",
    },
  ];
  const mockOnDelete = vi.fn();

  test("renders the Flow Board heading", () => {
    render(<List items={mockItems} onDelete={mockOnDelete} />);
    expect(
      screen.getByRole("heading", { level: 2, name: "Flow Board" })
    ).toBeInTheDocument();
  });

  test("renders correct number of ItemCards", () => {
    render(<List items={mockItems} onDelete={mockOnDelete} />);

    const cards = screen.getAllByRole("article");
    expect(cards).toHaveLength(2);
  });
  //
  test("renders empty grid when no items provided", () => {
    render(<List items={[]} onDelete={mockOnDelete} />);
    expect(screen.queryAllByRole("article")).toHaveLength(0);
  });
  // ALTERNATIVE if we want to test the number of items only in the component. Useful if there are other places where such items are rendered.

  test("ALTERNATIVE: renders correct number of ItemCards", () => {
    const { queryAllByRole } = render(
      <List items={mockItems} onDelete={mockOnDelete} />
    );
    expect(queryAllByRole("article")).toHaveLength(2);
  });

  test("ALTERNATIVE: renders empty grid when no items provided", () => {
    const { queryAllByRole } = render(
      <List items={[]} onDelete={mockOnDelete} />
    );
    expect(queryAllByRole("article")).toHaveLength(0);
  });
});
```

src/components/List.tsx

```tsx
import ItemCard from "./ItemCard";
import { type Item } from "../utils";

export default function List({
  items,
  onDelete,
}: {
  items: Item[];
  onDelete: (id: string) => void;
}) {
  return (
    <section className="mt-8">
      <h2 className="text-xl font-semibold mb-2">Flow Board</h2>
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        {items.map((item) => (
          <ItemCard key={item.id} {...item} onDelete={onDelete} />
        ))}
      </div>
    </section>
  );
}
```

## ItemCard Tests

components/ItemCard.tsx

```tsx
import { Trash2 } from "lucide-react";

type ItemCardProps = {
  id: string;
  title: string;
  description: string;
  category: string;
  onDelete: (id: string) => void;
};

const ItemCard = ({
  id,
  title,
  description,
  category,
  onDelete,
}: ItemCardProps) => {
  return <div>ItemCard</div>;
};
export default ItemCard;
```

- create `src/__tests__/ItemCard.test.tsx` file

```tsx
import { describe, test, vi } from "vitest";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import ItemCard from "../components/ItemCard";
import { type Item } from "../utils";

type MockProps = Item & { onDelete: () => void };

describe("ItemCard", () => {
  const mockProps: MockProps = {
    id: "1",
    title: "Test Task",
    description: "Test Description",
    category: "urgent",
    onDelete: vi.fn(),
  };

  test("renders card with correct content", () => {
    render(<ItemCard {...mockProps} />);

    expect(
      screen.getByRole("heading", { name: "Test Task" })
    ).toBeInTheDocument();
    expect(screen.getByRole("article")).toBeInTheDocument();
    expect(screen.getByText("Test Description")).toBeInTheDocument();
    expect(screen.getByText("urgent")).toBeInTheDocument();
  });

  test("calls onDelete when delete button is clicked", async () => {
    const user = userEvent.setup();
    render(<ItemCard {...mockProps} />);

    const deleteButton = screen.getByRole("button", {
      name: "Delete task: 1",
    });
    await user.click(deleteButton);

    expect(mockProps.onDelete).toHaveBeenCalledWith("1");
  });
});
```

## Context API

You can use current to setup to setup integration tests for App.tsx but in my case I will show you how to test components that use the context.

- create `src/AppWithContext.tsx` file

```tsx
const AppWithContext = () => {
  return <div>AppWithContext</div>;
};
export default AppWithContext;
```

- create `src/FlowContext.tsx` file

```tsx
import { createContext, useContext, ReactNode, useState } from "react";
import { Item, ItemWithoutId } from "./utils";

interface FlowContextType {
  items: Item[];
  handleAddItem: (newItem: ItemWithoutId) => void;
  handleDeleteItem: (id: string) => void;
}

const FlowContext = createContext<FlowContextType | undefined>(undefined);

export function FlowProvider({ children }: { children: ReactNode }) {
  const [items, setItems] = useState<Item[]>([]);

  const handleAddItem = (newItem: ItemWithoutId) => {
    setItems([...items, { ...newItem, id: Date.now().toString() }]);
  };

  const handleDeleteItem = (id: string) => {
    setItems((prev) => prev.filter((item) => item.id !== id));
  };

  return (
    <FlowContext.Provider value={{ items, handleAddItem, handleDeleteItem }}>
      {children}
    </FlowContext.Provider>
  );
}

export function useFlowContext() {
  const context = useContext(FlowContext);
  if (context === undefined) {
    throw new Error("useFlow must be used within a FlowProvider");
  }
  return context;
}
```

src/main.tsx

```tsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
// import App from './App.tsx';
import AppWithContext from "./AppWithContext.tsx";
import "./index.css";
import { FlowProvider } from "./FlowContext.tsx";

createRoot(document.getElementById("root")!).render(
  <StrictMode>
    <FlowProvider>
      <AppWithContext />
    </FlowProvider>
  </StrictMode>
);
```

src/AppWithContext.tsx

```tsx
import Form from "./components/Form";
import List from "./components/List";
import { useFlowContext } from "./FlowContext";
const AppWithContext = () => {
  const { items, handleAddItem, handleDeleteItem } = useFlowContext();
  return (
    <main className="container mx-auto p-4 max-w-6xl">
      <h1 className="text-3xl font-bold mb-8">Focus Flow</h1>
      <Form onSubmit={handleAddItem} />
      <List items={items} onDelete={handleDeleteItem} />
    </main>
  );
};
export default AppWithContext;
```

## AppWithContext Tests

- create `src/__tests__/AppWithContext.test.tsx` file

```tsx
import { render, screen } from "@testing-library/react";
import userEvent, { UserEvent } from "@testing-library/user-event";
import { describe, test, expect, beforeEach, vi } from "vitest";
import { FlowProvider } from "../FlowContext";
import AppWithContext from "../AppWithContext";

const getElements = () => ({
  titleInput: screen.getByRole("textbox", { name: /title/i }),
  descriptionInput: screen.getByRole("textbox", { name: /description/i }),
  categorySelect: screen.getByRole("combobox", { name: /category/i }),
  submitButton: screen.getByRole("button", { name: /add task/i }),
});

const customRenderAppWithContext = () => {
  return render(
    <FlowProvider>
      <AppWithContext />
    </FlowProvider>
  );
};

const addTestItem = async (user: UserEvent) => {
  const { titleInput, descriptionInput, categorySelect, submitButton } =
    getElements();
  await user.type(titleInput, "Test Item");
  await user.type(descriptionInput, "Test Content");
  await user.selectOptions(categorySelect, "urgent");
  await user.click(submitButton);
};

describe("AppWithContext", () => {
  let user: UserEvent;

  beforeEach(() => {
    vi.clearAllMocks();
    user = userEvent.setup();
    customRenderAppWithContext();
  });

  test("renders heading and form elements", () => {
    expect(
      screen.getByRole("heading", { level: 1, name: "Focus Flow" })
    ).toBeInTheDocument();
    // Verify all form elements are present
    const elements = getElements();
    Object.values(elements).forEach((element) => {
      expect(element).toBeInTheDocument();
    });
  });

  test("handles adding an item", async () => {
    const cardsBefore = screen.queryAllByRole("article");
    expect(cardsBefore).toHaveLength(0);

    await addTestItem(user);

    // Use getByText instead of findByText since we already have findAllByRole above
    const cardsAfter = await screen.findAllByRole("article");
    expect(cardsAfter).toHaveLength(1);
    expect(screen.getByText("Test Item")).toBeInTheDocument();
    expect(screen.getByText("Test Content")).toBeInTheDocument();
    expect(screen.getByText("urgent")).toBeInTheDocument();
  });

  test("handles deleting an item", async () => {
    await addTestItem(user);

    const deleteButton = screen.getByRole("button", { name: /delete/i });
    expect(deleteButton).toBeInTheDocument(); // Verify delete button exists

    await user.click(deleteButton);
    expect(screen.queryByText("Test Item")).not.toBeInTheDocument(); // Verify item content is gone
    expect(screen.queryAllByRole("article")).toHaveLength(0);
  });
});
```

## Conclusion

This project serves as a comprehensive guide to building a React application using Test-Driven Development (TDD). By following the steps outlined in this README, you can learn how to create a functional and testable application while gaining hands-on experience with React, TypeScript, and modern testing practices.

Feel free to extend the application with new features, improve the UI, or add more tests as you continue to learn and grow your skills in React development.

## Keywords

react, vite, tdd, test-driven development, testing, vitest, react testing library, tailwind css, typescript, productivity app, task management, context api

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
