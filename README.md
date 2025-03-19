import { FirstDataRenderedEvent } from 'ag-grid-community';

const onFirstDataRendered = useCallback((params: FirstDataRenderedEvent) => {
  params.api.forEachNode((node) => {
    node.setExpanded(node.id === '1');
  });
}, []);
