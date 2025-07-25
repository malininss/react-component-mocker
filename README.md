# React Component Mocker

Mock internal child components in unit tests with full type safety. Focus only on your main component’s logic.

[Vitest and Jest examples](examples/)

## Installation

```bash
npm i -D react-component-mocker
```

## Quick Start


```ts
import { createMockComponent, getMockComponentProps } from 'react-component-mocker';

vi.mock('./ChildComponent.tsx', () => ({
  ChildComponent: createMockComponent('child-component-mock')
}));


it('works with props correctly', () => {
  render(<MainComponent />);

  const child = screen.getByTestId('child-component-mock');

  // Generics are not necessary
  expect(child).toHaveProp<typeof ChildComponent>({ propA: 'value' });
  expect(child).toHaveProps<typeof ChildComponent>({
    propA: 'value',
    propB: 42,
  });

  // Calls functions passed as prop
  const props = getMockComponentProps<typeof ChildComponent>(child);

  // After call you could check the effect to the testing MainComponent
  props.onClick?.();
});
```

## API

### Component Mocks

- **`createMockComponent<T>(testId: string)`**  
  Returns a mock React component. Use inside `vi.mock()` or `jest.mock()`.

- **`getMockComponentProps<T>(element: HTMLElement)`**  
  Extracts props from the mocked component. Function props retain original references.

### Matchers

Available without setup (Vitest and Jest supported):

- **`toHaveProp(expected: Partial<T>)`**  
  Asserts that a component received a specific prop.

- **`toHaveProps(expected: Partial<T>)`**  
  Asserts that a component received a set of props.


## Jest Setup

This package is built as ES modules. Jest works with CommonJS by default, so you need to configure Jest to transform ES module code.

```ts
const config = {
  preset: 'ts-jest',
  transform: {
    // Allow ts-jest to handle .js files (this package is built as JS ES modules)
    '^.+\\.(ts|tsx|js|jsx)$': ['ts-jest', {
      tsconfig: 'tsconfig.test.json',
    }],
  },
  // By default, Jest ignores node_modules. Include this package for transformation
  transformIgnorePatterns: [
    'node_modules/(?!(react-component-mocker)/)'
  ],
};
```

## License

MIT
