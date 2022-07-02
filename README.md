# Inline boolean field for Laravel Nova 4

This package provides a field which allows updating booleans inline.

Example:

```
InlineBoolean::make('active')
    ->sortable()
    ->inlineOnIndex()
    ->showTextOnIndex()
    ->inlineOnDetail()
    ->trueText('Yes')
    ->falseText('No')
    ->enableMessage('The item is activated.')
    ->disableMessage('The item is deactivated.'),
```
