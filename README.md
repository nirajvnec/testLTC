console.log('Rendering async arrow for lazy-loading children');
return (
  <ArrowRight12px
    onClick={
      !params.disabled
        ? async () => {
            params.node.setDataValue('loading', true);
            params.api.onGroupExpandedOrCollapsed();
            await params.getAndAddChildren(params.data[params.idKey]);
            params.node.setDataValue('loading', false);
            params.node.expanded = true;
            params.api.onGroupExpandedOrCollapsed();
          }
        : undefined
    }
  />
);




console.log('Rendering simple expand arrow (children already loaded)');
return (
  <ArrowRight12px
    onClick={
      !params.disabled
        ? () => {
            params.node.expanded = true;
            params.api.onGroupExpandedOrCollapsed();
          }
        : undefined
    }
  />
);