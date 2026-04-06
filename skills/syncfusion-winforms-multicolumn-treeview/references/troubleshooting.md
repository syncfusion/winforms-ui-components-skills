# Troubleshooting Guide

This guide consolidates common issues and solutions for MultiColumnTreeView.

## Display Issues

**Subitems not visible:**
- Verify column count matches subitem count + 1
- Ensure columns were added before nodes
- Check subitems added in correct order

**Nodes not displaying hierarchy:**
- Verify nodes added to parent.Nodes collection
- Check if parent node is expanded
- Ensure ShowLines and ShowPlusMinus are true

**Images not showing:**
- Ensure image list assigned before setting indices
- Check if image indices are within bounds
- Verify images properly loaded into ImageList

**Border not visible:**
- Ensure BorderStyle is not None
- Check border color contrasts with background
- Verify BorderSides includes desired sides

## Performance Issues

**Poor performance with many nodes:**
- Use BeginUpdate/EndUpdate when adding nodes
- Set SuspendExpandRecalculate = true before expand operations
- Consider load on demand (BeforeExpand event)
- Avoid complex logic in frequently-fired events (Paint, HotTrack)

**Performance issues with large XML/JSON files:**
- Use BeginUpdate/EndUpdate during import
- Consider lazy loading for very large trees
- Show progress indicator for long operations
- Load data in chunks if possible

## Checkbox & Selection Issues

**CheckBoxes not updating parent:**
- Ensure InteractiveCheckBoxes = true at control level
- Or set InteractiveCheckBox = true on parent node

**Option buttons allow multiple selections:**
- Option buttons only enforce single selection within same parent
- Children of different parents can be selected independently

**Selection not working:**
- Check SelectionMode property
- Verify nodes are Enabled
- Check if BeforeSelect event is canceling selection

## Sorting & Filtering Issues

**Sorting not recursive:**
- Set SortOrder on child nodes
- Use recursive sorting method for all levels

**Filter not applying:**
- Verify RefreshFilter() called after setting Filter
- Check filter delegate returns correct boolean
- Test filter logic independently

**Filter showing no results:**
- Try FilterLevel = Extended if nodes not appearing
- Verify filter criteria isn't too restrictive
- Check if parent must match for children to be evaluated

## Event Issues

**Events not firing:**
- Verify event subscription
- Check if operation canceled in Before* event
- Ensure control properly initialized

**Multiple event firings:**
- Some events fire multiple times (NodeEditorValidateString)
- Use flags to prevent recursive handling
- Temporarily unsubscribe if needed

## Data Binding Issues

**XML parsing errors:**
- Ensure XML is well-formed
- Check for special characters needing escaping
- Validate XML structure before loading

**Missing nodes after import:**
- Verify recursive loading covers all child levels
- Check root node handling

**Subitems not loading from data:**
- Confirm column count matches before loading
- Ensure subitems read from data source

## Styling Issues

**Colors not applying:**
- Check if visual style overrides custom colors
- BackgroundColor takes precedence over BackColor
- Verify BrushInfo properly constructed

**Styles not visible:**
- Ensure Style property doesn't override custom settings
- Check Z-order and visibility of styled elements

## General Best Practices

1. Always add columns before nodes
2. Use BeginUpdate/EndUpdate for batch operations
3. Handle exceptions during file I/O
4. Test with large datasets early
5. Clear filters/sorters when not needed
6. Dispose resources properly on form close
7. Store references to data in Tag property
